# ADR-003: Deployment Architecture

Date: 2021-09-01
Status: Accepted
Author: CTO

## Context
We needed to ship fast. We are a 3-person engineering team and we only operate in Paris.

## Decision
Single VPS on Hetzner. One server runs everything: Node.js app, PostgreSQL, Redis, and Nginx.

## Rationale
- Cheapest option (~€80/month at the time).
- Simplest to manage — no infrastructure expertise needed.
- Single-city operation means latency is not a concern.

## Consequences
- Simple and cheap.
- No redundancy or disaster recovery.
- Should be revisited when we scale beyond Paris.
