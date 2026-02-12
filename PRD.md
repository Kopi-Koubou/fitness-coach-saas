# PRD: CoachKit — White-Label Client Management for Fitness Coaches

**Product:** CoachKit (working name)
**Type:** Micro-SaaS, B2B
**Author:** Kato (via Claude Code)
**Date:** 2026-02-11
**Target:** $5K MRR within 6 months
**Status:** Draft

---

## 1. Problem Statement

Online fitness coaching is a $10B+ market growing 30% YoY, yet most coaches cobble together 4–7 tools to run their business:

| Need | Current Solution | Pain |
|------|-----------------|------|
| Client management | Google Sheets / Notion | No automation, manual tracking |
| Workout programming | Excel / Trainerize ($$$) | Clunky, expensive, over-engineered |
| Progress tracking | WhatsApp photos / Notes app | Scattered, no trend analysis |
| Payments & scheduling | Calendly + Stripe manually | Disconnected, no unified view |
| Client communication | Instagram DMs / WhatsApp | Unprofessional, hard to scale past 15 clients |

**The core problem:** Coaches earning $3K–$15K/mo waste 8–12 hours/week on admin instead of coaching. Existing solutions (Trainerize, TrueCoach, PT Distinction) are either too expensive ($50–$150/mo), too complex, or designed for gym owners — not solo online coaches.

**Why now:** The creator-coach wave (IG/TikTok fitness influencers monetizing) has created a massive underserved segment of 500K+ solo coaches globally who need simple, affordable, brandable tools.

---

## 2. Solution Overview

**CoachKit** is a white-label client management platform built for solo online fitness coaches. Think "SimplePractice for trainers" — clean, mobile-first, affordable, and brandable.

**One-liner:** The all-in-one platform that lets fitness coaches manage clients, build workouts, and track progress — under their own brand.

### Design Philosophy

- **Warm and humanist** — not clinical blue SaaS. Think Apple Fitness+ energy, Fitbod's polish, FOMO-style warmth
- **Mobile-first** — coaches and clients live on their phones (iOS Safari primary)
- **Simple by default, powerful when needed** — progressive disclosure, not feature overload
- **White-label first** — every coach gets a branded experience for their clients

### Key Differentiators

1. **White-label from day one** — custom subdomain, logo, colors (competitors charge extra)
2. **AI workout generation** — coaches describe a goal, AI builds the program (saves 3+ hrs/week)
3. **Price** — $29/mo vs. $50–$150/mo competitors
4. **Built for solo coaches** — not gym owners, not enterprise

---

## 3. Target Users

### Primary: Solo Online Fitness Coach

- **Demographics:** 25–40, running coaching business via Instagram/TikTok
- **Revenue:** $3K–$15K/mo from 10–50 online clients
- **Tech comfort:** Uses Canva, Notion, IG confidently — not a developer
- **Pain:** Spending 8–12 hrs/week on admin, losing clients due to disorganized experience
- **Goal:** Look professional, scale to 50+ clients without hiring

### Secondary: Client (End User)

- **Demographics:** 20–45, paying $100–$300/mo for online coaching
- **Expectation:** Polished app experience, easy workout logging, clear progress
- **Comparison point:** MyFitnessPal, Apple Fitness+, Fitbod

---

## 4. Core Features

### 4.1 Client Management (MVP)

**Coach Dashboard:**
- Client roster with status tags (active, paused, prospect, offboarded)
- Per-client profile: goals, measurements, notes, check-in history
- Quick-action bar: assign workout, send message, log check-in
- Client onboarding flow with intake form (customizable questions)

**Client Portal (White-Label):**
- Branded login page at `coachname.coachkit.app` (or custom domain on Pro)
- Today view: current workout, upcoming check-in, recent messages
- Progress photos timeline with side-by-side comparison
- Measurement history with trend charts (weight, body fat, lifts)

### 4.2 Workout Builder

**Template System:**
- Drag-and-drop workout builder with exercise library (500+ exercises with video demos)
- Workout templates: create once, assign to many clients with per-client modifications
- Program builder: multi-week periodized programs (e.g., 12-week hypertrophy block)
- Superset/circuit/EMOM/AMRAP support

**Client Experience:**
- Clean workout view optimized for gym use (large text, rest timer, one-hand operation)
- Set logging: weight × reps with previous performance shown
- Exercise video demos (coach can upload custom demos)
- Workout completion tracking with coach notification

### 4.3 Progress Tracking

- **Check-in system:** Weekly/biweekly automated prompts (weight, photos, subjective feedback)
- **Measurement dashboard:** Weight, body measurements, lift PRs, adherence rate
- **Progress photos:** Upload with date tagging, side-by-side comparison tool
- **Coach analytics:** Client adherence heatmap, workout completion rates, average response time

### 4.4 AI Workout Generation

**Coach-Facing AI Assistant:**
- Input: client goal, experience level, available equipment, training days, injuries/limitations
- Output: Full periodized program (warm-up, working sets, RPE/RIR targets, deload weeks)
- Editable: AI generates draft, coach adjusts before assigning
- Learning: Saves coach preferences to improve future suggestions

**Implementation:**
- Claude API for program logic and exercise selection
- Structured output → validated against exercise database
- Prompt templates tuned per training style (powerlifting, bodybuilding, functional, etc.)
- Token cost: ~$0.02–$0.05 per generation (sustainable at scale)

### 4.5 Messaging (Post-MVP)

- In-app messaging between coach and client (replacing WhatsApp/DMs)
- Message templates for common responses (check-in feedback, program updates)
- Push notifications for new messages, workout assignments, check-in reminders

### 4.6 Payments Integration (Post-MVP)

- Stripe Connect for coach billing
- Recurring subscription management
- Invoice generation
- Revenue dashboard for coach

---

## 5. Tech Stack

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| **Frontend** | React 19, Next.js 15, TypeScript (strict) | SSR for SEO (landing page), fast client portal |
| **Styling** | Tailwind CSS v4, shadcn/ui | Rapid iteration, themeable for white-label |
| **Backend** | Supabase (Postgres + Auth + Storage + Realtime) | All-in-one, generous free tier, fast to ship |
| **Auth** | Supabase Auth | Multi-tenant: coach accounts + client accounts with RLS |
| **AI** | Claude API (Anthropic) | Best reasoning for complex program design |
| **File Storage** | Supabase Storage | Progress photos, exercise demo videos |
| **Payments** | Stripe (post-MVP) | Industry standard, Connect for marketplace model |
| **Hosting** | Vercel | Edge functions, easy preview deploys |
| **Analytics** | PostHog (self-hosted or cloud) | Product analytics, feature flags, session replay |
| **Email** | Resend | Transactional emails, check-in reminders |
| **Domain** | Wildcard DNS on `*.coachkit.app` | White-label subdomains |

### Database Architecture (Simplified)

```
organizations (coach accounts)
├── coaches (1:1 with org for now, multi-coach later)
├── clients
│   ├── measurements
│   ├── check_ins
│   ├── progress_photos
│   └── client_workout_logs
├── exercises (global + coach-custom)
├── workout_templates
│   └── template_exercises
├── programs
│   └── program_weeks
│       └── program_workouts
├── assigned_workouts
│   └── workout_sets (logged by client)
└── messages
```

**Multi-tenancy:** Row-Level Security (RLS) on all tables. Each coach is an org. Clients belong to one org. RLS policies ensure data isolation.

---

## 6. Pricing

### Tier Structure

| | **Starter** | **Pro** | **Scale** |
|---|---|---|---|
| **Price** | $29/mo | $59/mo | $99/mo |
| **Clients** | Up to 15 | Up to 50 | Unlimited |
| **Workout Builder** | Full | Full | Full |
| **Progress Tracking** | Full | Full | Full |
| **AI Workout Gen** | 10/mo | 50/mo | Unlimited |
| **White-Label** | Subdomain | Custom domain + branding | Custom domain + full CSS |
| **Check-in Automation** | Basic | Advanced (custom cadence) | Advanced |
| **Messaging** | — | In-app messaging | Priority support + messaging |
| **Payments/Billing** | — | Stripe integration | Stripe + revenue dashboard |
| **Video Demos** | Library only | Upload custom (5GB) | Upload custom (50GB) |

### Revenue Model

- **Target:** $5K MRR = ~100 Pro customers or ~170 Starter customers (blended)
- **LTV assumption:** 8-month average retention = $232–$472 per customer
- **CAC target:** < $50 (organic + outbound)
- **Gross margin:** ~85% (Supabase + Vercel + Claude API costs are minimal)

### Free Trial

- 14-day free trial of Pro tier, no credit card required
- Converts to Starter if no upgrade (keeps coaches engaged)

---

## 7. Distribution Strategy

### Channel 1: Instagram Outreach (Primary, Weeks 1–4)

**Target:** Fitness coaches with 1K–50K followers who post client transformations

**Playbook:**
1. Scrape coach accounts using hashtags: #onlinecoach, #fitnesscoach, #personaltrainer, #coachingbusiness
2. Filter for active coaches (posting client content, link-in-bio to coaching)
3. DM script (personalized):
   > "Hey [name], love the transformation you shared with [client]. Quick q — are you still using spreadsheets for programming? Built something that might save you 5+ hrs/week. Happy to give you free access to try it."
4. Volume: 30–50 personalized DMs/day
5. Conversion target: 3–5% reply rate → demo → 20% trial → 40% paid

### Channel 2: Content Marketing (Weeks 2–8)

- **SEO blog posts:** "Best tools for online fitness coaches 2026," "How to scale past 20 clients," "Workout programming templates"
- **YouTube/Loom demos:** 3-minute walkthrough videos targeting "[competitor] alternative" keywords
- **Twitter/X threads:** Building in public (indie hacker audience → coach referrals)

### Channel 3: Partnerships (Weeks 4–8)

- **Fitness certification programs:** Partner with NASM, ACE, ISSA for "recommended tools" placement
- **Coaching course creators:** Fitness business coaches (e.g., Online Trainer Academy) — affiliate deals
- **Supplement/apparel brands:** Co-marketing to their coach ambassador networks

### Channel 4: Community (Ongoing)

- **Free Slack/Discord community** for online fitness coaches (not product-gated)
- Content: business tips, programming discussions, monthly AMAs
- Organic product adoption through community trust

### Referral Program

- Coach refers another coach → both get 1 month free
- Clients who refer their coach → coach gets notified (social proof)

---

## 8. Competitive Landscape

| Product | Price | Strengths | Weaknesses |
|---------|-------|-----------|------------|
| **Trainerize** | $5–$150/mo | Established, app marketplace | Clunky UX, expensive at scale, gym-focused |
| **TrueCoach** | $19–$99/mo | Clean UX, coach-focused | No AI, limited white-label, no payments |
| **PT Distinction** | $20–$80/mo | Feature-rich | Dated UI, complex setup |
| **Google Sheets** | Free | Familiar | Not scalable, unprofessional |
| **CoachKit (us)** | $29–$99/mo | AI workout gen, white-label, modern UX, affordable | New, unproven, smaller exercise library |

**Our wedge:** AI workout generation + white-label at the lowest price point. No competitor offers both.

---

## 9. Success Metrics

### North Star

- **Monthly Recurring Revenue (MRR)** — target $5K within 6 months

### Leading Indicators

| Metric | Target (Month 1) | Target (Month 6) |
|--------|------------------|-------------------|
| Trial signups | 50 | 300/mo |
| Trial → Paid conversion | 25% | 35% |
| Paid customers | 12 | 110 |
| MRR | $500 | $5,000 |
| Monthly churn | <10% | <6% |
| Workouts assigned/coach/week | 5 | 15 |
| AI generations/coach/week | 3 | 8 |
| Client workout completion rate | 60% | 75% |

### Qualitative

- NPS > 50 within first 100 customers
- "Would be very disappointed" (PMF survey) > 40%

---

## 10. 30-Day Launch Plan

### Week 1: Foundation (Days 1–7)

| Day | Task |
|-----|------|
| 1–2 | Set up repo, Supabase project, Next.js scaffold, auth (coach signup + login) |
| 2–3 | Database schema: orgs, coaches, clients, exercises, workouts |
| 3–4 | Coach dashboard: client roster CRUD, client profile page |
| 5 | Client onboarding flow: intake form builder (3 default templates) |
| 6 | Exercise library: seed 200 exercises with metadata (muscle group, equipment, demo links) |
| 7 | Deploy to Vercel, wildcard DNS for subdomains, smoke test |

**Milestone:** Coach can sign up, add clients, view client profiles.

### Week 2: Workout Builder (Days 8–14)

| Day | Task |
|-----|------|
| 8–9 | Workout template builder: exercise selection, sets/reps/RPE, drag-and-drop ordering |
| 10 | Program builder: multi-week structure, assign template per day |
| 11 | Assign workout to client, client receives notification |
| 12 | Client workout view: today's workout, set logging (weight × reps), rest timer |
| 13 | AI workout generation: Claude API integration, prompt engineering, structured output |
| 14 | Polish: mobile optimization, loading states, error handling |

**Milestone:** Coach can build and assign workouts. Clients can log sets. AI generates programs.

### Week 3: Progress & White-Label (Days 15–21)

| Day | Task |
|-----|------|
| 15 | Check-in system: automated weekly prompts, coach review queue |
| 16 | Progress photos: upload, date tag, side-by-side comparison |
| 17 | Measurement tracking: weight chart, body measurement trends |
| 18 | Coach analytics: client adherence, workout completion heatmap |
| 19 | White-label: subdomain routing, coach logo/color upload, themed client portal |
| 20 | Landing page: marketing site with pricing, demo video, testimonials placeholder |
| 21 | Stripe integration: subscription checkout for coach accounts |

**Milestone:** Full MVP loop — onboard client, assign workouts, track progress, branded experience.

### Week 4: Launch & Outreach (Days 22–30)

| Day | Task |
|-----|------|
| 22 | QA pass: full user journey testing (coach + client), mobile Safari testing |
| 23 | Seed 5 beta coaches (personal network / fitness communities), collect feedback |
| 24–25 | Fix critical bugs from beta feedback, polish rough edges |
| 26 | Write launch content: Product Hunt draft, Twitter thread, 3 blog posts |
| 27–28 | Instagram outreach begins: 30–50 DMs/day to target coaches |
| 29 | Product Hunt launch + Twitter/X announcement |
| 30 | Analyze first-week metrics, prioritize feedback, plan Month 2 |

**Milestone:** Live product with 10+ trial signups, 2–3 paying customers, outreach machine running.

---

## 11. Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Coaches won't switch from spreadsheets | Medium | High | Free migration service for first 50 coaches; show time-savings calculator |
| AI workout quality insufficient | Medium | Medium | Human-in-the-loop: AI generates draft, coach always edits before assigning |
| Exercise video library too thin | High | Medium | Partner with open-source exercise databases; allow coach uploads from day 1 |
| Churn from solo coaches going out of business | Medium | Medium | Target coaches with 10+ clients (established); pricing low enough to keep |
| Competitor copies AI feature | Low (short-term) | Medium | Ship fast, build brand, community moat |

---

## 12. Future Roadmap (Post-MVP)

**Month 2–3:**
- In-app messaging (replace WhatsApp)
- Nutrition tracking (meal plan templates, macro tracking)
- iOS native wrapper (PWA → native shell for App Store presence)

**Month 4–6:**
- Stripe Connect for client payments
- Group coaching support (shared programs for cohorts)
- Marketplace: coaches sell program templates to other coaches
- Zapier/webhook integrations

**Month 7–12:**
- Native mobile apps (React Native)
- AI form check via video upload
- White-label mobile app (coach's own App Store listing)
- Team plans (coaching businesses with multiple trainers)

---

## Appendix: Design References

- **Apple Fitness+** — clean, motivational, warm color palette
- **Fitbod** — excellent workout logging UX, progressive disclosure
- **FOMO app** — warm, humanist design language (not cold SaaS blue)
- **Linear** — dashboard clarity, keyboard shortcuts, fast interactions
- **SimplePractice** — gold standard for practitioner SaaS UX

---

*Generated by Kato via Claude Code — 2026-02-11*
