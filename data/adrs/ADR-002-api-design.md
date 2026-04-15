# ADR-002: API Design Style

Date: 2022-03-10
Status: Accepted
Author: Lead Backend Developer

## Context
Our mobile apps (iOS, Android) and web app all need to communicate with the backend.
The team debated REST, GraphQL, and a custom JSON-RPC protocol before the first public beta.

## Options Considered

1. **REST over HTTPS** — well-understood, broad tooling support, HTTP caching works out of the box.
2. **GraphQL** — flexible queries, reduces over-fetching, but more complex server-side implementation and harder to cache at the CDN layer.
3. **Custom JSON-RPC** — full control, but no ecosystem, high maintenance burden.

## Decision
REST API with JSON payloads over HTTPS, versioned under `/api/v1/`.
All endpoints follow standard HTTP verb semantics (GET, POST, PATCH, DELETE).

## Rationale
- Every team member has REST experience; no learning curve.
- Cloudflare CDN can cache GET responses transparently.
- Third-party restaurant POS integrations expect standard REST — easier partner onboarding.
- Our current traffic volume does not justify the operational overhead of GraphQL.

## Consequences

**Positive**
- Simple, predictable API surface; easy to document.
- New developers onboard quickly.
- Restaurant POS integrations require no custom SDK.

**Negative**
- Mobile clients sometimes need 3–4 round trips to render a single screen (order details + courier + restaurant + items).
- Breaking changes require explicit versioning (`/api/v2/`); no per-field evolution.

**To watch**
- If mobile round-trip latency becomes a user complaint, add a GraphQL layer for the highest-traffic composite queries without replacing the REST API.
