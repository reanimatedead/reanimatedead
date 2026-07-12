# Hi, I'm Takahiro (@reanimatedead)

I build **data pipelines and AI tooling where governance is part of the
architecture, not an afterthought** — PII controls that run before persistence,
quality checks that actually stop a pipeline, and documentation that states
limitations honestly instead of overselling.

## Featured projects

| Project | What it is |
|---|---|
| [**edinet-mcp**](https://github.com/reanimatedead/edinet-mcp) | MCP server for Japanese company financials (FSA EDINET). ~3,600 listed companies, normalized to English keys and JPY. No API key, no signup. Runtime schema validation and data-freshness warnings built in. |
| [**bpo-contact-center-intelligence-pipeline**](https://github.com/reanimatedead/bpo-contact-center-intelligence-pipeline) | GRC-first contact-center ETL (synthetic data). Keyed HMAC pseudonymization and a fail-closed quality gate placed *before* the warehouse — the ordering is the control. |

## What I care about

- **Controls as code** — a report nobody must act on is not a control; a gate with a non-zero exit code is.
- **Honest threat models** — pseudonymization is not anonymization, and READMEs should say so.
- **Primary sources** — numbers come from filings and official data, with attribution, or they are marked as missing.

## Stack

Python (pandas, pytest) · TypeScript (Node, MCP SDK, Ajv, Vitest) · GitHub Actions · SQLite
