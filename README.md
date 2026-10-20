# DebateLab

> **Archived student project.** This repository is a snapshot of DebateLab as it stood in May 2026. It was built as a student project between March and May 2026 and is open-sourced for portfolio purposes. It is no longer maintained, and issues or pull requests will not be reviewed.

DebateLab is an AI debate-practice web app for high school students. Students pick a motion, deliver a timed speech into the microphone, and get a live transcript plus structured, rubric-based AI feedback. They can also run a full 5-round debate against an AI opponent, challenge another student to a 1v1 duel judged by AI, chat with an AI debate coach, and work through structured debate courses. The interface is available in English and Vietnamese.

## Features

- **Solo practice:** choose from a topic library across several categories, pick a side, and run prep and speech timers for single-speech or speaking practice.
- **Full-round debates vs AI:** a 5-round flow (opening, AI rebuttal, counter-rebuttal, AI closing, closing) with three AI difficulty levels.
- **Live speech transcription:** real-time speech-to-text via Deepgram streaming, with optional text-to-speech for AI speeches.
- **AI judging and feedback:** Google Gemini scores each speech on Content, Structure, Language, and Persuasiveness, with band descriptors, an annotated transcript, and concrete "try this" suggestions.
- **Duel mode:** 1v1 debates between two students via share code or matchmaking, with ready checks, per-round speech submission, integrity checks, and an AI verdict with a clash map of answered, dropped, and turned arguments.
- **AI debate coach:** a streaming chat assistant (Groq-hosted Llama models) for explaining formats, structuring openings and rebuttals, brainstorming arguments, and reviewing past sessions.
- **Courses:** structured learning paths with articles, videos, quizzes, and practice activities, plus an admin course builder.
- **Gamification:** XP and levels, daily streaks, achievements, credits, and referral rewards.
- **History and analytics:** session history with filters, detailed per-session review, skill snapshots, and progress charts.
- **Admin tools:** user analytics, class and club management with attendance and schedules, duel moderation, and in-app "smart popup" campaigns.
- **Localization:** English and Vietnamese UI via `next-intl`.
- **Auth:** email/password and Google OAuth through Supabase Auth, with row-level security on all tables.

## Tech Stack

- **Framework:** Next.js 16 (App Router) with React 19 and TypeScript
- **Backend:** Supabase (PostgreSQL, Auth, Row Level Security, RPCs)
- **AI:** Google Gemini (speech analysis, AI opponent rebuttals, duel judging), Groq (AI coach chat)
- **Speech:** Deepgram (streaming transcription and text-to-speech)
- **Styling:** Tailwind CSS v4, shadcn/ui, Base UI
- **State and data:** Zustand, SWR
- **Animation and visuals:** Framer Motion, Lottie, Recharts
- **Content:** react-markdown, remark-gfm, KaTeX, Mermaid
- **i18n:** next-intl (English, Vietnamese)
- **Analytics (optional):** PostHog, Vercel Analytics and Speed Insights

## Getting Started

### Prerequisites

- Node.js 20 or newer
- A [Supabase](https://supabase.com) project
- API keys for [Google AI Studio](https://aistudio.google.com/apikey) (Gemini), [Deepgram](https://console.deepgram.com/), and [Groq](https://console.groq.com/keys)

### 1. Install dependencies

```bash
npm ci
```

### 2. Configure environment variables

```bash
cp .env.example .env.local
```

Then fill in `.env.local`. The required values are:

| Variable | Purpose |
|----------|---------|
| `NEXT_PUBLIC_SUPABASE_URL` | Supabase project URL |
| `NEXT_PUBLIC_SUPABASE_ANON_KEY` | Supabase anon (public) key |
| `SUPABASE_SERVICE_ROLE_KEY` | Server-side admin operations and seeding |
| `GEMINI_API_KEY` | AI feedback, AI opponent, duel judging |
| `DEEPGRAM_API_KEY` | Speech transcription and text-to-speech |
| `GROQ_API_KEY` | AI coach chat |

Optional values (`GEMINI_MODEL`, `GROQ_CHAT_MODEL`, `NEXT_PUBLIC_POSTHOG_KEY`, `CRON_SECRET`, `BETA_ALL_ACCESS`, `SMART_POPUPS_ENABLED`, `DEV_ADMIN_BYPASS`, and others) are documented in `.env.example`.

### 3. Set up the database

Apply the SQL files in `supabase/migrations/` to your Supabase project in filename order, either with the Supabase CLI (`supabase db push`) or by running them in the SQL editor. Migration numbers have a gap at `011`, which is intentional.

Seed the starter courses:

```bash
npx tsx src/lib/seed/run-seed.ts
```

### 4. Run the app

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000). A microphone and a Chromium-based browser give the best experience.

### Other scripts

```bash
npm run build              # production build
npm run lint               # ESLint
npm run test:entitlements  # one of several tsx-based unit test scripts, see package.json
```

## Project Structure

```
src/
  app/
    [locale]/            # Localized pages (landing, auth, onboarding, protected app)
      (protected)/       # Dashboard, practice, history, debates (duels), courses, chat, profile, settings, admin
    api/                 # Route handlers: analyze, rebuttal, chat, tts, deepgram-token, debate-duels, analytics, cron
    actions/             # Server actions
  components/            # Feature components (practice, feedback, debates, chat, courses, admin, landing) and shadcn/ui
  hooks/                 # Speech recognition, TTS, countdown, data hooks
  i18n/                  # next-intl config and en/vi message catalogs
  lib/                   # Gemini prompts, Supabase clients, analytics, entitlements, seed data, topic library
  stores/, store/        # Zustand stores
  types/                 # Shared TypeScript types
supabase/migrations/     # Database schema, RLS policies, and RPCs
design-artifacts/        # UI design references for the feedback and duel review screens
design.md                # Visual design system notes
```

## Scoring Rubric

| Category | Max Score | Sub-categories |
|----------|-----------|----------------|
| Content & Argumentation | 40 | Claim Clarity, Evidence, Logic, Counter-Arguments |
| Structure & Organization | 25 | Introduction, Body, Conclusion |
| Language & Delivery | 25 | Vocabulary, Grammar, Fluency |
| Persuasiveness | 10 | Audience Awareness, Impactfulness |

Bands: Expert (85 to 100), Proficient (70 to 84), Competent (50 to 69), Developing (30 to 49), Novice (0 to 29).

## Notes on This Snapshot

- Names and emails in admin QA fixtures are fictional demo data.
- No hosted demo is provided. To try the app, run it locally with your own API keys.

## Archived

This project is archived and provided as-is for reference. It is not maintained, and dependencies, APIs, and model names may be out of date.

## License

Released under the MIT License. See [LICENSE](LICENSE).
