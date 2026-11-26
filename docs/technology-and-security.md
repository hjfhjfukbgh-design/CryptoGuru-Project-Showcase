# CryptoGuru — Technology, Security & Operational Considerations

> This document summarizes technologies and security-relevant design areas visible in the reviewed codebase. It is not a penetration-test report, security certification or confirmation that production infrastructure is correctly configured.

## 1. Technology stack

| Area | Technology / integration | Role |
|---|---|---|
| Backend | TypeScript | API and application services |
| Frontend | JavaScript, HTML, CSS | Telegram Mini App |
| Serverless runtime | Cloudflare Workers | Request handling and serverless execution |
| Stateful services | Cloudflare Durable Objects | Coordinated application and trading state |
| Persistence | Durable Object SQLite, Cloudflare KV | Structured and key-value state |
| Messaging / client | Telegram Bot API and Telegram Mini Apps | User interaction |
| AI | Gemini API | Companion responses and AI-assisted decisions |
| Market data | Hyperliquid REST / WebSocket interfaces | Market context and data access |
| External trading integration | Walt MCP, OAuth 2.1 / PKCE | External account and execution integration groundwork |
| Testing | Vitest, Miniflare | Unit and worker-environment testing |
| CI/CD | GitHub Actions | Validation and engineering automation |

The precise versions, current dependency tree, deployment bindings and active external services should be confirmed from the source and target environment during due diligence.

## 2. Security-relevant implementation areas

The reviewed codebase includes work in the following areas:
- OAuth 2.1 / PKCE integration.
- Session and request validation.
- Idempotency and duplicate-operation protection.
- Trading risk limits and kill-switch concepts.
- Quote freshness and structured input validation.
- Fixed-point arithmetic for financial calculations.
- Persistent state and recovery-oriented logic.
- Workflow permissions and task coordination.

The existence of these controls in source code does not establish that every path is secure, correctly configured or enabled in a deployed environment.

## 3. Known security and operational caveats

### LIVE trading controls
The reviewed application explicitly indicated that the live accounting/risk stack was not wired. The live close path and account creation also had blocking behavior. The live trading track must remain classified as unavailable for real-money use until the integration is completed and independently tested.

### AI-generated output
AI output must be treated as untrusted input. It should never bypass deterministic validation, position sizing, exposure limits or authorization checks. AI responses can be incorrect, incomplete or malformed.

### Market data
Market data may be delayed, incomplete or unavailable. Trading decisions should reject stale or invalid quotes rather than silently using them. Data-source integrity and timestamp semantics require testing.

### Secrets and credentials
API keys, OAuth client secrets, Telegram tokens, AI credentials and provider credentials must be stored in an appropriate secret manager or protected deployment configuration. They must not be committed to the public showcase repository, included in screenshots, or transferred without an explicit credential-rotation plan.

### Serverless configuration
Cloudflare bindings, Durable Object migrations, environment variables, access policies and production routes must be reviewed in the target account. Repository code alone cannot prove the state of deployed infrastructure.

### CI/CD automation
GitHub Actions workers require carefully scoped repository permissions and protected secrets. Workflow automation should be reviewed for untrusted input, prompt injection, token exposure, excessive permissions and unintended repository modifications.

## 4. Data protection questions for due diligence

A buyer should establish:
1. What user data is collected and where it is stored.
2. Which data is sent to Gemini or other external providers.
3. How long conversation and attachment data is retained.
4. Whether users can delete their data.
5. Which services and subprocessors receive data.
6. How production secrets are stored, accessed and rotated.
7. Whether logs contain personal, account or credential information.
8. What backup, export and deletion procedures exist.

These questions require inspection of actual deployment settings and service terms; this document does not assert a particular compliance status.

## 5. Recommended pre-transfer security checklist

- [ ] Run dependency and vulnerability scans against the exact transfer commit.
- [ ] Search the full Git history for accidentally committed secrets.
- [ ] Rotate all credentials after transfer.
- [ ] Remove access for former users and automation identities where appropriate.
- [ ] Review GitHub Actions permissions and third-party actions.
- [ ] Review OAuth redirect URIs, scopes, token storage and revocation.
- [ ] Review authorization checks on every trading and account endpoint.
- [ ] Test replay, duplicate, timeout and ambiguous-provider-response behavior.
- [ ] Validate accounting invariants and risk-limit enforcement.
- [ ] Confirm production data retention and deletion procedures.
- [ ] Perform an independent security review before any live trading activation.

## 6. Operational readiness evidence

The following evidence should be provided separately if available:
- Current deployment identifiers and release commit.
- Redacted Cloudflare configuration and binding inventory.
- Current CI run and test report.
- Monitoring and alerting configuration.
- Incident and rollback procedures.
- Provider sandbox test records.
- Security review findings and remediation status.

## 7. Security position

CryptoGuru is a software project with security-related engineering controls under development. No claim is made here that it has passed an independent penetration test, obtained regulatory approval, achieved a particular compliance certification, or is safe for real-money trading.

---
*Keep this document synchronized with the current source and deployment state. Do not publish credentials, private endpoints or user data in the public showcase.*
