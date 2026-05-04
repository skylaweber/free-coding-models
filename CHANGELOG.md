## [0.3.55] - 2026-05-04

### Added

- **GitHub Models provider** — Added GitHub Models as an active OpenAI-compatible source using `GITHUB_TOKEN`, with a curated free-tier catalog covering GPT-4.1, DeepSeek, Llama, and Mistral families.

### Changed

- **Free provider catalog refreshed** — Rebuilt the active catalog to 172 models across 15 vetted providers, prioritizing providers with practical recurring free or free-limited usage.
- **Stale model IDs replaced** — Updated NVIDIA NIM, OpenRouter, Google/Gemini, Qwen DashScope, ZAI, Cloudflare Workers AI, OVHcloud, SambaNova, Scaleway, and OpenCode Zen model IDs from the current audit.
- **Trial-only providers removed from the active catalog** — Removed providers that are paid-only, shutdown, trial-credit-only, too tiny to be useful, or unclear as stable free APIs.
- **Codestral key handling modernized** — `MISTRAL_API_KEY` is now the primary env var for Codestral/Mistral launches, while `CODESTRAL_API_KEY` remains accepted as an input alias.
- **Provider filters now derive from the catalog** — The command palette provider filter no longer hardcodes stale provider keys, so future catalog changes automatically appear in the filter list.
- **README refreshed** — Updated provider counts, model counts, setup guidance, provider caveats, OpenCode Zen models, Gemini CLI models, and licensing notes to match the current catalog.

### Fixed

- **Router defaults no longer pin stale NVIDIA models** — The Smart Router now starts from current NVIDIA model IDs instead of removed Kimi/GLM/DeepSeek IDs.
- **OpenCode and Kilo handle new OpenAI-compatible providers** — Generic provider config generation now supports newly added providers such as GitHub Models without adding one-off launcher branches.
