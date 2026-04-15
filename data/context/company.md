# Foodi — Company Context

## What We Do
Foodi is a food delivery marketplace connecting restaurants, couriers, and customers.
Founded in Paris in 2021, currently operating in Paris only.

## Current Numbers
- 500 active restaurants
- 2,200 registered couriers
- ~1,800 daily orders (peak: 3,000 on weekends)
- Average order value: €28
- Commission: 15% from restaurants + €2.50 delivery fee from customers
- Monthly recurring revenue: ~€285,000

## Growth Trajectory
- Month-over-month order growth: 38%
- Customer base doubles every 4 months
- Planned launches: London (Q3 this year), Berlin (Q4), Amsterdam (Q1 next year)

## Current Technology Stack
- Backend: Node.js monolith (Express.js), ~45,000 lines of code
- Database: Single PostgreSQL 14 instance on Hetzner Frankfurt (32 vCPU, 256 GB RAM)
- Cache: Redis 7 (single instance, same VPS)
- Frontend: React web app + React Native iOS/Android
- Hosting: Single VPS, Hetzner Cloud — €240/month
- CDN: Cloudflare (free tier)
- Payments: Stripe
- SMS: Twilio

## Engineering Team
- 4 backend developers (Node.js, some Python)
- 2 frontend developers (React, React Native)
- 1 DevOps engineer
- No dedicated SRE or data engineering

## Infrastructure Budget
- Current spend: ~€800/month (VPS + SaaS)
- Approved budget for scaling: up to €5,000/month

## Known Pain Points (Business Impact)

1. **Peak-hour crashes** — App goes down 12:00–13:30 and 19:00–21:00 on busy days.
   Estimated lost revenue: ~€4,200/month (15% of orders during those windows).

2. **Restaurant onboarding takes 2 weeks** — Menu import is manual.
   Competitors onboard in 2 days. We are losing restaurant sign-ups.

3. **Courier GPS staleness** — Location updates every 30 seconds.
   Customers complain about "ghost" couriers and call support.

4. **Order status race conditions** — Orders occasionally show wrong status
   (e.g., "delivered" before picked up). Root cause: no event ordering guarantees.

5. **No multi-region capability** — Serving London/Berlin/Amsterdam from a single
   Paris VPS means >200 ms latency and no local data residency for GDPR.

6. **Single point of failure** — No redundancy, no DR plan.
   Six outages in the last 8 months; longest was 6 hours. Each hour costs ~€595 in lost GMV.

## Regulatory Constraints
- GDPR: EU customer data must remain in EU; UK data post-Brexit requires adequacy decision handling.
- PSD2: Payment flows must comply with strong customer authentication (SCA) rules.
- UK: ICO registration required for London launch.

## Timeline
- London launch: Q3 (4 months away) — hard deadline, marketing committed.
- Berlin launch: Q4 (8 months away).
- Amsterdam: Q1 next year.
