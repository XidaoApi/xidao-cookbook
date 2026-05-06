# XiDao Cookbook


[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE) [![GitHub release](https://img.shields.io/github/v/release/XidaoApi/xidao-cookbook)](https://github.com/XidaoApi/xidao-cookbook/releases) [![GitHub stars](https://img.shields.io/github/stars/XidaoApi/xidao-cookbook?style=social)](https://github.com/XidaoApi/xidao-cookbook/stargazers)


Practical migration guides and production patterns for teams using an OpenAI-compatible AI API.

## Why this repo exists

Most AI integration content stops at the first successful request.
Production teams usually need help with harder problems:
- safe provider switching
- phased rollout and regression testing
- fallback and routing strategy
- cost control by workload
- observability when traffic spans multiple model options

This cookbook focuses on those higher-intent, production-facing problems first.

## Sections

### 1. Migration and rollout
- switch from OpenAI API with minimal code changes
- verify which SDK assumptions your app depends on
- run rollout checklists before moving real production traffic
- separate output-quality risk from integration-risk during migration

### 2. Reliability and routing
- fallback versus blind retry behavior
- health-aware routing patterns
- latency-tier and task-tier model selection
- staged rollout patterns that reduce outage blast radius

### 3. Cost optimization
- prompt trimming
- model routing by task value
- caching repeated requests
- token and request visibility for cost analysis

### 4. Framework examples
- Python OpenAI SDK
- Node.js OpenAI SDK
- LangChain-ready notes
- Vercel AI SDK integration notes

## Recommended first documents
- `guides/migrate-from-openai.md`
- `guides/openai-compatible-rollout-checklist.md`
- `guides/reduce-api-costs.md`
- `guides/openrouter-alternative.md`
- `guides/llm-failover-routing-patterns.md`
- `guides/vercel-ai-sdk-openai-compatible.md`

## Repo relationship map
- `xidao-python-examples` → minimal Python usage examples
- `xidao-nodejs-examples` → minimal Node.js usage examples
- `llm-failover-router-demo` → code-first reliability and routing examples
- `xidao-cookbook` → migration, rollout, routing, and cost guides

## Links
- Website: https://global.xidao.online/
- Support: support@xidao.online
- Telegram: https://t.me/ccyu085
