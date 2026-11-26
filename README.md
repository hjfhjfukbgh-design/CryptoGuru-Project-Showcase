# CryptoGuru

**CryptoGuru** is a Telegram-based crypto trading product combining a Mini App, virtual trading infrastructure, AI-assisted workflows and an integration layer for external trading services.

This repository is the **public project showcase** for evaluation by prospective buyers and partners. It intentionally does not contain the private source code.

## Product overview

CryptoGuru brings together three distinct trading tracks:

| Track | Purpose | Current positioning |
|---|---|---|
| DEMO / Grid | Rule-based virtual grid trading with simulated positions and risk controls | One of the more mature product components; deployment and live operational status require independent verification |
| AI PAPER | AI-generated trading decisions and a virtual-trading lifecycle | Under active engineering; end-to-end autonomous operation and risk controls require further validation |
| LIVE / Walt | Integration path for external account and execution services | Not enabled for real-money trading; production readiness is not claimed |

The product also includes a Telegram Mini App, AI Companion, backend services, a trading engine, and GitHub-based engineering automation.

## Main capabilities

- Telegram bot and Mini App user experience.
- Virtual trading primitives, including positions, PnL, margin, fees, slippage and simulated liquidation.
- Grid strategy configuration and risk limits.
- AI-assisted market analysis and structured trading decisions.
- AI Companion modes for general assistance, learning and research.
- Persistent account, conversation and trading state.
- Integration groundwork for external trading services.
- Automated tests and GitHub Actions workflows.

## Architecture and technology

The codebase uses TypeScript and JavaScript, with Cloudflare Workers, Durable Objects, KV and SQLite for serverless application infrastructure. The product integrates with Telegram Mini Apps and Bot API, Gemini for AI functionality, Hyperliquid market data interfaces, and a Walt MCP/OAuth integration layer.

See:
- [Architecture](docs/architecture.md)
- [Feature status](docs/feature-status.md)
- [Technology and security](docs/technology-and-security.md)
- [Acquisition and due diligence](docs/acquisition.md)

## Development status

CryptoGuru is an actively developed software project, not a finished, independently certified trading platform.

Important distinctions:
- Implemented code does not by itself prove a feature is deployed or operational.
- Virtual trading is not equivalent to exchange execution.
- AI-generated decisions are not a guarantee of profitable performance.
- The LIVE/Walt path is not represented as ready for real funds.
- Test results must be tied to a specific commit and CI run; historical green checks are not a guarantee of current status.

The feature-status document describes implemented areas, open engineering work and known limitations. Status should be revalidated against the private repository's current default branch before any transaction.

## Source code and transaction

The production source repository is private:

- Private repository: `hjfhjfukbgh-design/walletbot-mcp`
- Public showcase: this repository

Access to source code, technical handover, intellectual-property scope, third-party dependencies and transfer terms should be agreed as part of a formal due-diligence process. This public repository is an overview, not a source-code release.

## Ownership and third-party rights

Project authorship and ownership are represented by the seller as **Kirill Rodionov**. Any acquisition should independently review commit history, contributor activity, dependency licenses, third-party assets, service terms and any other rights that may affect transferability. This statement is not a substitute for legal due diligence.

## Disclaimer

CryptoGuru is software infrastructure and is not financial, investment or legal advice. No profitability, trading performance, uptime, regulatory status or suitability for real-money trading is promised. Cryptocurrency trading involves substantial risk, including the possible loss of capital.
