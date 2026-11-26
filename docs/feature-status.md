# CryptoGuru — Feature Status & Engineering Roadmap

> Status snapshot based on the engineering review available on 26 November 2026. Treat percentages as directional engineering estimates, not audited completion metrics. Revalidate every item against the current private repository and deployed environment before relying on it.

## 1. Executive status

CryptoGuru contains substantial implemented application and trading-simulation code. Its maturity differs significantly by subsystem.

The most important distinction is between:
- **Implemented:** code exists in the repository.
- **Integrated:** components are connected through the application.
- **Tested:** automated or manual tests demonstrate expected behavior.
- **Deployed:** a current environment is running the relevant version.
- **Production-ready:** security, reliability, operational controls and failure handling have been validated.

A feature should not be described as production-ready merely because its implementation exists.

## 2. Subsystem overview

| Subsystem | Code-level maturity (indicative) | Operational maturity (indicative) | Current assessment |
|---|---:|---:|---|
| Core architecture and API | 80–85% | 50–60% | Substantial implementation; deployment and runtime evidence require refresh |
| Telegram Mini App | 75–85% | 50–60% | Broad interface coverage; complete live integration needs verification |
| DEMO / Grid | 75–85% | 55–65% | One of the more mature trading components; production smoke test not established |
| Virtual Trading Engine | 70–80% | 45–55% | Significant simulation and accounting logic; live execution is outside this claim |
| AI Companion | 75–85% | 50–60% | Core conversational workflows implemented; provider configuration and deployed behavior unverified |
| AI PAPER | 40–50% | 20–30% | Decision-generation groundwork exists; complete autonomous lifecycle remains under development |
| LIVE / Walt | 25–35% | 0% | Integration groundwork only; not ready for real funds |
| Engineering worker automation | 70–80% | 0–15% | Workflow definitions exist; reliable current execution and credentials are unverified |
| Risk and accounting | 65–75% | 45–55% | Important controls exist, but open issues and validation work remain |
| Documentation and handover | 45–55% | 35–45% | Public overview is being assembled; technical handover package still needs completion |

These ranges are subjective estimates based on code review, not measurements produced by a formal certification process. They must not be averaged into a claim that the entire product is a particular percentage complete.

## 3. DEMO / Grid

### Implemented areas
- Grid strategy and level management.
- Virtual positions and order lifecycle.
- LONG-side strategy behavior.
- Take-profit and stop-loss logic.
- Range and recentering behavior.
- Configurable exposure and loss limits.

### Relevant engineering work
Merged PR #2412 addressed the daily-loss baseline across restarts, pause/resume behavior, DEMO re-selection and reset handling, including unrealized PnL checks on each tick.

### Remaining validation
- Run current unit and integration suites against the current default branch.
- Verify runtime configuration and risk limits in the target environment.
- Perform a documented end-to-end DEMO smoke test.
- Confirm persistence and recovery behavior under restart and partial-failure conditions.

**Assessment:** comparatively mature as a virtual-trading feature, but deployment and operational readiness are not independently confirmed.

## 4. Virtual Trading Engine

### Implemented areas
The engine includes virtual position opening and closing, LONG/SHORT handling, PnL and margin calculations, fee and slippage simulation, virtual liquidation, TP/SL handling, idempotency, concurrency-related protections and persistence-oriented logic. Fixed-point money arithmetic is implemented in a dedicated module.

### Remaining validation
- Review accounting invariants and rounding boundaries.
- Run restart/recovery and duplicate-request tests.
- Validate concurrency and idempotency under realistic load.
- Confirm that all strategy paths use the same validated accounting primitives.
- Keep simulated fills and balances clearly separated from external exchange execution.

**Assessment:** meaningful simulation infrastructure; not evidence of a live trading venue or profitable strategy.

## 5. AI PAPER

### Implemented areas
- Gemini-backed decision generation.
- Market context collection.
- Structured decision output.
- Decision persistence.
- Configured request limits and throttling.

The reviewed configuration included up to 10 markets, global and per-user daily limits, and a per-user request interval. Exact limits should be rechecked in the current source before being presented as current product behavior.

### Open engineering items
- Draft PR #2503: lifecycle work for AI PAPER, including a broader decision-to-position flow. It was not confirmed merged in the reviewed snapshot and was based on an older main revision.
- Issue #2528 identified a notional/leverage guard problem: validation could check margin while failing to constrain the effective notional exposure.
- Draft PR #2529 proposed a correction; it must be reviewed, merged and tested before the related control is described as fixed.
- Funding/accounting and quote-validation work remained distributed across draft PRs and issues.
- Full signal → deterministic risk validation → open → manage → close behavior was not confirmed complete on the default branch.

### Required next steps
1. Rebase and reconcile the lifecycle work with current main.
2. Fix and test notional/leverage enforcement.
3. Complete funding and accounting semantics.
4. Validate quote freshness and source integrity.
5. Add lifecycle tests covering failure, retry, restart and duplicate events.
6. Demonstrate the complete flow in a controlled paper environment.

**Assessment:** promising development track, but not a finished autonomous trading product.

## 6. AI Companion

### Implemented areas
- General, Learning and Research modes.
- Persistent conversations and message history.
- Context assembly and account-related read-only context.
- Attachment handling for supported image, audio and video inputs.
- Request coordination, retries and idempotency-related safeguards.
- Quotas and message-size controls.

### Remaining validation
- Confirm Gemini credentials and deployed provider connectivity.
- Test attachment limits and failure handling in the deployed environment.
- Review privacy, retention and data-access behavior.
- Verify that read-only account context cannot trigger trading actions.
- Add operational monitoring and user-facing error reporting.

**Assessment:** substantial feature implementation; current production operation and provider configuration are unverified.

## 7. Telegram Mini App

### Implemented areas
The interface includes Wallet, Trading, Agents and AI Companion areas, plus market selection, position/history presentation, charts, themes, language support and Telegram-oriented interaction behavior.

### Known limitations
- Wallet UI is not proof of a self-custody wallet.
- The Agents screen is not proof that autonomous agents are connected and operating.
- Chart-related work includes merged and draft changes; complete provider integration must be verified.
- Current public availability and end-to-end behavior of the deployed Mini App have not been confirmed.

### Remaining work
- Run browser/device smoke tests inside Telegram.
- Verify API contracts for every screen.
- Check loading, empty, error and reconnect states.
- Validate chart data source, timestamps and stale-data handling.
- Review responsive behavior and accessibility.

## 8. LIVE / Walt

### Current position
**Not ready for real-money trading.**

The codebase contains OAuth 2.1 / PKCE and MCP-oriented integration work, account and position access paths, preview and execution-related code, confirmation tokens and selected validation gates.

However, the reviewed implementation also contained material blockers:
- The live accounting/risk stack was explicitly not wired.
- The live close endpoint returned HTTP 409.
- Account creation was blocked in the reviewed code path.
- Provider schema and real-response behavior were not fully verified.
- Full close semantics and provider idempotency guarantees remained unresolved.
- A safe, complete end-to-end sandbox flow was not established.
- Telegram Mini App OAuth callback work remained open.

Relevant reviewed work included Issue #2514 (provider schema and real responses), Draft PR #2509 (OAuth hardening), and additional Walt-related draft work.

### Release blockers
1. Verify exact provider API and MCP schemas against real responses.
2. Complete and independently review accounting and risk integration.
3. Implement and test safe close behavior.
4. Verify idempotency and ambiguous-result recovery.
5. Validate kill switch and all live gates.
6. Test OAuth callback and session lifecycle.
7. Run full end-to-end tests in a provider-approved sandbox.
8. Obtain a separate security review before enabling real funds.

## 9. Engineering worker automation

The repository includes GitHub Actions workflows for a role-based worker pool described as 14 workers (A–N), coordinated through GitHub issues and finite workflow jobs.

This is **workflow-driven automation**, not a set of permanently running agents.

The reviewed snapshot showed:
- Scheduled and event-driven workflow execution.
- Lease and timeout configuration.
- Shared issue-based task coordination.
- Some workflow runs failing before their first visible steps.
- A documentation mismatch between the worker count described in older docs and the workflow definitions.
- Required model credentials not independently confirmed.

### Remaining work
- Resolve failures occurring before job steps.
- Validate repository and workflow permissions.
- Confirm all required secrets exist without exposing them.
- Align documentation with actual worker definitions.
- Add observability, retry policy and dead-letter handling.
- Demonstrate successful task completion from issue creation through reviewed pull request.

## 10. CI and test evidence

The last known green result in the reviewed material was associated with merged PR #2412: 27 test files and 392 tests passed, TypeScript checks passed, and Mini App syntax checks passed; deployment was skipped.

This is historical evidence for that specific change. It does not prove that the current default branch passes all tests or that production is deployed.

Before a sale or technical demonstration:
- Run the complete current test suite.
- Run type checks, linting and build validation.
- Record the exact commit SHA and CI run URL.
- Explain skipped checks and deployment steps.
- Run a separate end-to-end smoke test.

## 11. Suggested engineering sequence

### Priority 0 — Establish a trustworthy baseline
- Freeze and identify the current main commit.
- Re-run all CI checks.
- Inventory open PRs, stale branches and failing workflows.
- Confirm current deployment state without exposing secrets.
- Produce a reproducible demo environment.

### Priority 1 — Make virtual trading demonstrable
- Validate Grid and Trading Engine accounting.
- Add restart, concurrency and failure-recovery tests.
- Verify Mini App-to-backend integration.
- Record a controlled DEMO walkthrough.

### Priority 2 — Complete AI PAPER safely
- Rebase lifecycle work.
- Fix notional/leverage guard.
- Complete accounting, quote validation and TP/SL semantics.
- Demonstrate the full paper lifecycle.

### Priority 3 — Stabilize AI Companion and automation
- Verify provider setup and attachment handling.
- Improve error visibility and monitoring.
- Repair GitHub Actions worker execution.
- Align documentation with actual implementation.

### Priority 4 — Treat LIVE as a separate release project
- Complete provider contract validation, risk integration and close semantics.
- Conduct independent security review.
- Keep live execution disabled until all gates are satisfied.

## 12. Buyer-facing summary

CryptoGuru should be presented as a substantial, multi-component crypto application with a comparatively mature virtual-trading foundation and active AI, Mini App and external-integration development.

It should **not** be marketed as:
- A proven profitable trading system.
- A fully autonomous live trading platform.
- A production-ready exchange or custody service.
- A deployed system with verified uptime or active customers unless separate evidence is supplied.

The most credible demonstration is a controlled walkthrough of the Mini App, DEMO/Grid behavior, virtual Trading Engine and AI Companion, accompanied by a pinned commit and current CI evidence.
