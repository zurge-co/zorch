# Zorch Documentation

> Central documentation hub for the Zorch AI Key Orchestration Platform.

Zorch is a production-ready AI Key Orchestration Platform built in Rust. It exposes a single OpenAI-compatible API that routes requests across multiple upstream providers (OpenAI, Anthropic, and any OpenAI-compatible backend) with per-key governance, dynamic pricing, billing, analytics, a sandboxed middleware engine, and a full admin dashboard.

## Documentation Map

| Document | What you'll learn |
|---|---|
| [Root README](../README.md) | Full project overview, quick start, architecture, API reference, and deployment |
| [Middleware Guide](./middleware.md) | How to write, bind, and audit Rhai middleware scripts |
| [Provider Setup](./provider-setup.md) | How to configure upstream providers, auth types, target models, and pricing |
| [Security Notice](./security.md) | Encryption defaults, inspector capture levels, and pre-launch hardening checklist |
| [Project State](./project_state.yaml) | Machine-readable canonical feature status and roadmap |

## Quick Start

See the [Quick Start section in the root README](../README.md#quick-start) for prerequisites, Docker Compose setup, migrations, and your first API key.

## Current Feature Status

This summary is derived from [`docs/project_state.yaml`](./project_state.yaml).

### Completed

- **Proxy**: pass-through to OpenAI/Anthropic providers, multi-key rotation, streaming, retry, model aliases, sticky routing, circuit breaker, request-phase Rhai middleware, access windows, and per-key governance (RPM/RPD/budget/allowlist).
- **Admin Dashboard**: API key CRUD, provider CRUD, model alias CRUD, pricing CRUD, middleware admin, per-key middleware binding, analytics charts, dashboard metrics, and settings view.
- **Analytics**: PostgreSQL `requests_log`, Prometheus `/metrics`, and cost attribution by tag.
- **Security**: AES-256-GCM provider key encryption, SHA-256 client key hashing, admin secret / scoped bearer auth, and constant-time comparison.

### Partial / Missing

- `clickhouse-inspector` — capture works, admin reads are not yet wired.
- `middleware-response-phases` — `response.pre_client` and `inspector.pre_capture` phases exist in types but are not invoked in the proxy pipeline.
- `real-time-updates` — dashboard has no WebSocket or SSE live updates.

## Roadmap / Next Recommended

The following items are tracked in [`docs/project_state.yaml`](./project_state.yaml) as the highest-value next steps:

1. **Dynamic Pricing Engine** — load `provider_model_config` from the DB at startup instead of using hardcoded prices.  
   Full plan: [`docs/plans/dynamic-pricing-engine.md`](./plans/dynamic-pricing-engine.md)
2. **Persist Gateway Rejections** — write `BillingRecord` rows for rate-limit, budget, model-allowlist, circuit-breaker, and middleware blocks so they appear in analytics.  
   Full plan: [`docs/plans/persist-gateway-rejections.md`](./plans/persist-gateway-rejections.md)
3. **Add Integration Tests** — HTTP-level E2E tests for proxy and admin endpoints.
4. **Wire Response Middleware** — implement `response.pre_client` and `inspector.pre_capture` phases.
5. **ClickHouse Analytics Reads** — add admin endpoints that query ClickHouse for analytics.

## Recent Changes

- **2026-07-13** — Replaced plugin-based middleware with a sandboxed Rhai scripting engine; per-API-key binding, configurable sandbox limits, validate/dry-run in admin.
- **2026-07-13** — Added model alias routing with priority-based multi-target resolution and sticky target-key routing.
- **2026-07-13** — Added provider/gateway latency breakdown to billing records.
- **2026-07-13** — Expanded admin dashboard with model management, provider detail pages, middleware Monaco editor, settings page, and API key middleware binding.
- **2026-06-18** — Enforced per-key governance limits (RPM/RPD/budget/model-allowlist) in the proxy pipeline; added admin dashboard UI and backend validation.

## Keeping Documentation Up to Date

If you add a new guide or plan under `docs/`, please link it in this README so the navigation stays current. For code contributions, see the [Contributing section in the root README](../README.md#contributing).
