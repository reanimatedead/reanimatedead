# Hi, I'm Takahiro (@reanimatedead)

I build **data pipelines and AI tooling where governance is part of the
architecture, not an afterthought** — PII controls that run before persistence,
quality checks that actually stop a pipeline, and documentation that states
limitations honestly instead of overselling.

## Featured projects

| Project | What it is |
|---|---|
| [**Wikipedia attention archive**](https://huggingface.co/datasets/nevirim/wikipedia-attention-13lang) (Hugging Face dataset) | The 1,000 most-viewed Wikipedia articles per day in 13 languages, from July 2015 to yesterday. A Mac mini at home collects it every morning. It also lists topics that jumped abroad while Japanese Wikipedia stayed quiet. CC0. |
| [**hf-signal-dashboard**](https://github.com/reanimatedead/hf-signal-dashboard) · [live page](https://reanimatedead.github.io/hf-signal-dashboard/) | Buy/sell markers for every Nikkei 225 and Nasdaq 100 stock, each printed next to how that marker did in a backtest. Public data only (Yahoo, SEC EDGAR). Not investment advice. |
| [**taka-pixel-reference**](https://github.com/reanimatedead/taka-pixel-reference) · [live page](https://reanimatedead.github.io/taka-pixel-reference/) | A one-page reference for Pixel phones, in Japanese: update deadlines, known bugs per Android version, recovery steps. |
| [**edinet-mcp**](https://github.com/reanimatedead/edinet-mcp) | Retired in July 2026. It served Japanese company financials to MCP clients; a free service (edinetdb.jp) now covers the same ground, so I stopped maintaining it. Left up as a record. |

## What I care about

- **Controls as code** — a report nobody must act on is not a control; a gate with a non-zero exit code is.
- **Honest threat models** — pseudonymization is not anonymization, and READMEs should say so.
- **Primary sources** — numbers come from filings and official data, with attribution, or they are marked as missing.

## Stack

Python (pandas, pytest) · TypeScript (Node, MCP SDK, Ajv, Vitest) · GitHub Actions · SQLite
