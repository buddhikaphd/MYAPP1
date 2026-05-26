# SkinBitz — Developer Setup Guide

## Prerequisites

| Tool | Version | Install |
|---|---|---|
| Node.js | 22 LTS | https://nodejs.org |
| Docker Desktop | Latest | https://docker.com |
| Expo CLI | Latest | `npm i -g expo-cli` |
| Git | Latest | https://git-scm.com |

---

## 1. Clone & Install

```bash
# From MYAPP1/ root

# Install backend dependencies
cd skinbitz-backend
npm install

# Install mobile dependencies
cd ../skinbitz-mobile
npm install
```

---

## 2. Environment Setup

```bash
# Backend
cd skinbitz-backend
cp .env.example .env
# Edit .env with your API keys (see required keys below)

# Mobile
cd ../skinbitz-mobile
cp .env.example .env
```

### Required API Keys (minimum for local dev)

| Key | Where to get | Required |
|---|---|---|
| `OPENAI_API_KEY` | platform.openai.com | YES |
| `ANTHROPIC_API_KEY` | console.anthropic.com | YES |
| `JWT_SECRET` | Generate (see below) | YES |
| `STRIPE_SECRET_KEY` | dashboard.stripe.com | For payment features |
| `GOOGLE_CLIENT_ID` | console.cloud.google.com | For Google login |

Generate JWT secret:
```bash
node -e "console.log(require('crypto').randomBytes(64).toString('hex'))"
```

---

## 3. Start Backend Infrastructure

```bash
cd skinbitz-backend

# Start PostgreSQL + Redis
docker-compose up -d postgres redis

# Wait ~10 seconds for DB to be ready, then:

# Run database migrations
npx prisma migrate dev --name init

# Generate Prisma client
npx prisma generate

# (Optional) Seed ingredient database
npm run prisma:seed

# Start the API
npm run start:dev
```

API will be available at:
- GraphQL Playground: http://localhost:3000/graphql
- Health check:       http://localhost:3000/health
- REST scan endpoint: http://localhost:3000/api/v1/scan/image

---

## 4. Start Mobile App

```bash
cd skinbitz-mobile

# Start Expo dev server
npm start

# Then press:
# i → iOS Simulator
# a → Android Emulator
# Scan QR code → Physical device (Expo Go app)
```

---

## 5. Quick Test Flow

1. Open app → Sign up with email
2. Complete skin profile (5 onboarding screens)
3. Go to Scan tab
4. Paste this test ingredient list:
   ```
   Aqua, Glycerin, Niacinamide, Ceramide NP, Ceramide AP, Ceramide EOP,
   Cholesterol, Fatty Acids, Hyaluronic Acid, Panthenol, Tocopherol
   ```
5. Tap "Analyze Ingredients"
6. See compatibility score + AI recommendation

---

## 6. Project Structure

```
MYAPP1/
├── skinbitz-mobile/          # React Native Expo app
│   ├── app/                  # Expo Router screens
│   │   ├── (auth)/           # Login, signup, onboarding
│   │   └── (tabs)/           # Home, Scan, Coach, Progress, Profile
│   ├── components/ui/        # Reusable UI components
│   ├── stores/               # Zustand state management
│   ├── services/             # Apollo GraphQL + REST API calls
│   ├── types/                # TypeScript types
│   └── theme/                # Colors, design tokens
│
├── skinbitz-backend/         # NestJS API
│   ├── src/
│   │   ├── auth/             # JWT auth + strategies
│   │   ├── user/             # User management
│   │   ├── skin-profile/     # Skin profile CRUD
│   │   ├── ingredient/       # INCI engine + safety scoring
│   │   ├── scan/             # Barcode + OCR + analysis
│   │   ├── ai/               # OpenAI + Claude integration
│   │   ├── subscription/     # Stripe payments
│   │   ├── progress/         # Skin log tracking
│   │   └── health/           # Health check endpoints
│   ├── prisma/               # Database schema + migrations
│   ├── docker-compose.yml
│   ├── Dockerfile
│   └── .env.example
│
└── docs/                     # Architecture documentation
    ├── SKINBITZ_PRD.md
    ├── SKINBITZ_ARCHITECTURE.md
    ├── SKINBITZ_DATABASE_SCHEMA.md
    ├── SKINBITZ_API_DESIGN.md
    ├── SKINBITZ_AI_PROMPTS.md
    ├── SKINBITZ_SECURITY_DEPLOYMENT.md
    └── SKINBITZ_ROADMAP_MONETIZATION.md
```

---

## 7. Key API Calls

### Signup
```graphql
mutation {
  signup(input: {
    email: "test@example.com"
    password: "SecurePass123"
    acceptedTerms: true
    acceptedPrivacyPolicy: true
  }) {
    accessToken
    refreshToken
    user { id email role }
  }
}
```

### Analyze Ingredients
```graphql
mutation {
  scanText(text: "Aqua, Glycerin, Niacinamide, Ceramide NP") {
    compatibility {
      overall
      label
      breakdown { skinTypeMatch allergenSafety }
    }
    summary
    warnings { type severity message }
  }
}
```

### Start AI Coach
```graphql
mutation {
  startConversation(sessionType: GENERAL) {
    id
    messages { role content }
  }
}
```

---

## 8. Common Issues

| Issue | Fix |
|---|---|
| `prisma generate` fails | Run `npm install` first |
| Port 3000 in use | `lsof -i :3000` then kill process |
| Expo app can't connect to API | Check `EXPO_PUBLIC_API_URL` points to your machine IP (not localhost) on physical device |
| OpenAI quota exceeded | Check usage at platform.openai.com |
| Camera not working in simulator | Use text paste or file upload instead |

---

## 9. Adding Ingredients to Database

The ingredient database is seeded from `prisma/seed/`. To add real ingredient data:

1. Export from EWG Skin Deep database (public research API)
2. Import CosIng EU ingredient list (CSV download from ec.europa.eu)
3. Run: `npm run prisma:seed`

A starter seed with 50 common ingredients is included.
