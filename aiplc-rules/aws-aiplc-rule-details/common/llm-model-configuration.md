# LLM Model Configuration Guide

## Purpose
Single source of truth for choosing the LLM model, region, and request parameters for prototypes that call an LLM (agentic, or an application with AI features). Other rule files refer to this file instead of naming model IDs themselves.

## Principles

1. **Never hardcode a model ID from memory.** Model versions are retired over time and model IDs differ by region. Resolve the model ID at build time (see below), confirm it with the user, and record it in the prototype's `iteration-log.md`.
2. **Default model family**: For AWS Bedrock and Anthropic, the default is the **newest active Claude Sonnet model**. The user can choose another model.
3. **Do not set sampling parameters for Claude models.** Do not pass `temperature`, `top_p`, or `top_k` — current Claude models reject non-default values with a validation error. For other providers, leave sampling at the provider default unless the user asks.
4. **Default `max_tokens`**: `16000`. Lower values can truncate responses, because current models count reasoning tokens against the limit.

## AWS Bedrock

### Resolve the Model ID at Build Time

Bedrock on-demand calls to Claude models use an **inference profile ID**, not the bare model ID. Profiles have a prefix that depends on the region: a geographic prefix (for example `us.`, `eu.`, `apac.`) or `global.`. A profile that exists in one region may not exist in another — for example, `us.` profiles are not available in `ap-northeast-2` (Seoul).

1. **Region**: Use the region the user chose (ask if not set). Do not assume `us-west-2`.
2. **If the AWS CLI is available and credentials are configured** (resolve after the credential step in `discovery/prototype-building.md`), list what the region actually offers. Add `--profile <name>` when the user uses an AWS CLI profile:

```bash
# Active Anthropic models in the region
aws bedrock list-foundation-models --region <region> [--profile <name>] --by-provider Anthropic \
  --query "modelSummaries[?modelLifecycle.status=='ACTIVE'].modelId" --output text

# Inference profiles available in the region
aws bedrock list-inference-profiles --region <region> [--profile <name>] --type-equals SYSTEM_DEFINED \
  --query "inferenceProfileSummaries[?contains(inferenceProfileId, 'anthropic.claude-sonnet')].inferenceProfileId" --output text
```

   - Pick the highest-versioned **ACTIVE** Sonnet model (for example `sonnet-5-5` is newer than `sonnet-5`, which is newer than `sonnet-4-6`). Never pick a model whose lifecycle status is `LEGACY`.
   - Use the inference profile for that model with the region's geographic prefix if one is listed; otherwise use the `global.` profile and tell the user that global profiles can route requests to other geographies.
3. **If the CLI is not available** (some IDEs, sandboxes, or hosts without AWS tooling), the listing fails (for example, the credentials only work with a Bedrock API key, which these commands do not use, or the IAM policy does not allow listing), or the user runs the prototype themselves: ask the user for the inference profile ID in a question file. Tell them where to find it: Amazon Bedrock console → the region they use → Cross-region inference (inference profiles). Show the examples below for the expected format.
4. Present the resolved ID with the other defaults and let the user confirm or change it.

### Example IDs (Last verified: 2026-10-26)

Examples only — always resolve the current ID with the steps above.

| Region | Example inference profile ID |
|---|---|
| `us-west-2` | `us.anthropic.claude-sonnet-5-5` or `global.anthropic.claude-sonnet-5-5` |
| `ap-northeast-2` | `global.anthropic.claude-sonnet-5-5` (no `us.` profiles in this region) |

### API Credentials

Credentials must be visible to the AI tool's process when it starts — see Step 2.5 of `discovery/prototype-building.md`. Prefer temporary credentials (see `common/security-guardrails.md`).

**Option 1: AWS CLI profile with temporary credentials (Recommended)**
```bash
aws configure sso
aws sso login --profile your-profile
```
- Works without putting secrets in environment variables; pass the profile name to the prototype (`AWS_PROFILE`, or `boto3.Session(profile_name=...)`)
- Check with `aws sts get-caller-identity --profile your-profile`

**Option 2: Amazon Bedrock API key (short-term)**
```bash
export AWS_BEARER_TOKEN_BEDROCK=bedrock-api-key-...
```
- Get from: Amazon Bedrock console → API keys

**Option 3: Temporary access keys**
```bash
export AWS_ACCESS_KEY_ID=your_access_key
export AWS_SECRET_ACCESS_KEY=your_secret_key
export AWS_SESSION_TOKEN=your_session_token
export AWS_REGION=<region>
```
- Copy short-term keys from your AWS access portal; avoid long-lived IAM user access keys

**Option 4: Instance or container role**
- On EC2, Cloud9, SageMaker, or a dev container with an attached role, the SDK finds credentials on its own — no variables or profile needed
- Check with `aws sts get-caller-identity`; the role must allow invoking the model

### Python Code Example (Strands SDK)
```python
from strands.models import BedrockModel

model = BedrockModel(
    model_id="<inference-profile-id resolved above>",
    region_name="<region>",
    max_tokens=16000,
)
```

### Common Errors and Solutions

**Error: "This model version has reached the end of its life"** (or the model is listed as `LEGACY`)
- Cause: The model ID was hardcoded or copied from an older example
- Solution: Resolve the model ID again with the steps above and pick an ACTIVE model

**Error: "Invocation of model ID ... with on-demand throughput isn't supported"**
- Cause: Using the bare model ID instead of an inference profile ID
- Solution: Use the inference profile ID listed for your region (geographic prefix or `global.`)

**Error: "The provided model identifier is invalid"** for an ID with a prefix
- Cause: The profile prefix does not exist in this region (for example `us.` in `ap-northeast-2`)
- Solution: List the region's inference profiles and use one that exists there

**Error: "ValidationException" mentioning `temperature`, `top_p`, or `top_k`**
- Cause: Sampling parameters sent to a Claude model that accepts only the defaults
- Solution: Remove those parameters

**Error: "ResourceNotFoundException" or "Access denied"**
- Cause: Model access not granted for this account and region, or the IAM policy does not allow invoking the model
- Solution: Check model access in the Amazon Bedrock console for the region, and the IAM permissions for `bedrock:InvokeModel*`

## Anthropic Direct

### Model ID
Use the newest Claude Sonnet model available to the user's API key. Find it without printing the key:

```bash
curl -s https://api.anthropic.com/v1/models -H "x-api-key: $ANTHROPIC_API_KEY" -H "anthropic-version: 2023-06-01"
```

If that is not possible, ask the user for the model ID and point them to https://platform.claude.com/docs/en/about-claude/models/overview (do not fetch it yourself unless the user asks — see `common/security-guardrails.md`). Example, last verified 2026-10-26: `claude-sonnet-5-5`. Confirm with the user.

### API Credentials
```bash
export ANTHROPIC_API_KEY=your_anthropic_api_key
```
- Get from: https://console.anthropic.com/

### Python Code Example
```python
from strands.models.anthropic import AnthropicModel
import os

model = AnthropicModel(
    client_args={"api_key": os.environ["ANTHROPIC_API_KEY"]},
    model_id="<model-id confirmed with the user>",
    max_tokens=16000,
)
```

## Other Providers (OpenAI, Google Gemini, Meta Llama, Others)

Model names for these providers change frequently and are not tracked in this file. Ask the user which model to use, and point them to the provider's current model list:

| Provider | Model list | Environment Variable | Get Key From |
|----------|-----------|---------------------|--------------|
| OpenAI | https://platform.openai.com/docs/models | `OPENAI_API_KEY` | platform.openai.com/api-keys |
| Google Gemini | https://ai.google.dev/gemini-api/docs/models | `GOOGLE_API_KEY` | aistudio.google.com/apikey |
| Meta Llama | https://llama.developer.meta.com/docs/models | `LLAMA_API_KEY` | llama.developer.meta.com |

### Python Code Examples (Strands SDK)
```python
import os

# OpenAI
from strands.models.openai import OpenAIModel
model = OpenAIModel(
    client_args={"api_key": os.environ["OPENAI_API_KEY"]},
    model_id="<model-id confirmed with the user>",
)

# Google Gemini
from strands.models.gemini import GeminiModel
model = GeminiModel(
    client_args={"api_key": os.environ["GOOGLE_API_KEY"]},
    model_id="<model-id confirmed with the user>",
)

# Meta Llama
from strands.models.llamaapi import LlamaAPIModel
model = LlamaAPIModel(
    client_args={"api_key": os.environ["LLAMA_API_KEY"]},
    model_id="<model-id confirmed with the user>",
)
```

## Quick Reference Table

| Provider | Model ID | Environment Variable | Get Key From |
|----------|----------|---------------------|--------------|
| AWS Bedrock | Resolved per region at build time (see above) | `AWS_PROFILE` (SSO), `AWS_BEARER_TOKEN_BEDROCK`, or `AWS_ACCESS_KEY_ID` + `AWS_SECRET_ACCESS_KEY` + `AWS_SESSION_TOKEN` | AWS access portal / Bedrock console |
| Anthropic | Newest Claude Sonnet, confirmed with the user | `ANTHROPIC_API_KEY` | console.anthropic.com |
| OpenAI / Gemini / Llama | Asked from the user | See table above | See table above |

## Workflow Integration

When building prototypes in the Discovery phase:

1. **Always ask for LLM provider** (even if default is Bedrock), and the region for Bedrock
2. **Check for API credentials** and request them upfront if missing (don't wait until build fails)
3. **Resolve the model ID** for the provider and region using this file, with those credentials — never from memory
4. **Verify credentials and the model ID work** with a minimal call before proceeding with the prototype build
5. **Record** the provider, region, and model ID as the first entry of the prototype's `iteration-log.md` (create it if it does not exist yet) — the same place on every path

This ensures smooth prototype building without credential or model errors mid-process.
