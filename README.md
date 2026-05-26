# SkinBitz — AI-Powered Skincare Intelligence Platform

> "The AI dermatologist in your pocket."

SkinBitz enables users to scan cosmetic ingredient labels and receive scientifically accurate, personalized skincare analysis based on their skin profile, environment, and goals.

---

## Documentation Index

| Document | Contents |
|---|---|
| [PRD](docs/SKINBITZ_PRD.md) | Product requirements, personas, features, success metrics |
| [Architecture](docs/SKINBITZ_ARCHITECTURE.md) | System diagram, frontend/backend structure, AI pipeline, scoring algorithm |
| [Database Schema](docs/SKINBITZ_DATABASE_SCHEMA.md) | Full Prisma schema (PostgreSQL 16 + pgvector) |
| [API Design](docs/SKINBITZ_API_DESIGN.md) | GraphQL schema, REST endpoints, rate limiting, example payloads |
| [AI Prompts](docs/SKINBITZ_AI_PROMPTS.md) | All LLM system prompts, RAG pipeline, OCR extraction, conflict detection |
| [Security & Deployment](docs/SKINBITZ_SECURITY_DEPLOYMENT.md) | HIPAA-aware design, Docker, CI/CD, Kubernetes, testing strategy |
| [Roadmap & Monetization](docs/SKINBITZ_ROADMAP_MONETIZATION.md) | MVP scope, phased roadmap, pricing, launch strategy, UI flows |

---

## Tech Stack Summary

| Layer | Technology |
|---|---|
| Mobile | React Native + Expo SDK 52, TypeScript, NativeWind, Zustand, TanStack Query |
| Backend | NestJS 10, GraphQL (code-first), Prisma 5, PostgreSQL 16 |
| AI | OpenAI GPT-4o (recommendations), Claude API (coaching), RAG/pgvector |
| Infrastructure | AWS ECS/EKS, Redis 7, S3, CloudFront, GitHub Actions CI/CD |
| Auth | Firebase Auth, JWT HS512, refresh token rotation |
| Payments | Stripe, Apple IAP, Google Play Billing |
| Observability | Pino, OpenTelemetry, Mixpanel, PostHog |

---

## MVP Scope (Months 1–4)

1. Email + social auth
2. Skin profile builder (5 screens)
3. Barcode + OCR + manual ingredient scanning
4. INCI ingredient engine (safety, comedogenicity, allergens)
5. AI compatibility score (0–100) + personalized explanation
6. Free (5 scans/mo) + Premium ($9.99/mo) subscription

See [Roadmap](docs/SKINBITZ_ROADMAP_MONETIZATION.md) for full phased plan.

---

## Medical Disclaimer

All AI analysis in SkinBitz is for **informational purposes only** and does not constitute medical advice. Users should consult a licensed dermatologist for diagnosis and treatment of skin conditions.

---

*Architecture version 1.0.0 | Generated 2026-05-25*
