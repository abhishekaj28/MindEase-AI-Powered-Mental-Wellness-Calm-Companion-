# MindEase - Calm Companion

A hackathon prototype of a gentle mental wellness web app: a mood check-in, a weekly mood dashboard, a supportive chat companion, and guided breathing exercises.

> **Important disclaimer:** MindEase is **not a medical device**, is not a diagnostic or treatment tool, and is **not a substitute for professional mental health care, therapy, or emergency services**. If you are in crisis, or think you may harm yourself or others, contact your local emergency number or a qualified crisis line or professional right away. The app does not currently provide crisis resources or escalation of any kind.

## Honest status: is the AI real?

**No. There is no real AI or LLM in this project.** The code contains no API calls, no AI SDK, no API keys and no backend. The "AI companion" in the Chat page is a rule-based script (`src/hooks/useChat.ts`):

- Your message is lowercased and matched against hard-coded keyword lists (for example "sad", "stress", "angry", "hopeless").
- A canned reply is then picked at random from predefined response lists, with a simple conversation state machine that can suggest the breathing exercise.
- A 1-2 second delay is added on purpose to simulate "thinking".
- Matching is naive substring matching, so it can misread messages (for example "no" or "hi" appearing inside other words). It does not understand context.

Despite the name, treat the chat as a scripted prototype only.

## What is implemented

| Feature | Status |
|---|---|
| Daily mood check-in (happy / okay / sad / stressed) with optional reflection | Working; saved in the browser's `localStorage` (key `mindease_moods`) |
| "Your Journey" dashboard: 7-day mood chart and weekly counts | Working, computed from locally stored check-ins (Recharts) |
| Chat companion | Working UI, **scripted/keyword-based replies (mock)** |
| Calm Mode breathing: Calm (4-4-6) and Box (4-4-4-4) | Working animated guide |
| Mood sounds | Short tones generated in-browser with the Web Audio API (no audio files) |

All data stays in your browser. There is no account system, server, database or analytics in the code.

## Tech stack

From `package.json`:

- React 18, TypeScript, Vite 5 (`@vitejs/plugin-react-swc`)
- Tailwind CSS 3 with `tailwindcss-animate` and `@tailwindcss/typography`
- shadcn/ui (Radix UI primitives), `lucide-react` icons
- React Router 6, TanStack React Query (set up but not used for any network calls), React Hook Form + Zod
- Framer Motion, Recharts, Sonner toasts
- ESLint 9 with typescript-eslint
- Scaffolded with Lovable (`lovable-tagger` dev dependency)

## Run locally

Requires Node.js and npm.

```sh
npm install
npm run dev        # start the dev server (Vite prints the local URL)
npm run build      # production build into dist/
npm run preview    # serve the production build locally
npm run lint       # run ESLint
```

`npm install` and `npm run build` were run successfully during the writing of this README (Vite warns that the main JS chunk is over 500 kB). `npm run lint` and `build:dev` were not run. No environment variables or API keys are needed.

## Project structure

```
src/
  pages/        Index (check-in), Dashboard, Chat, CalmMode, NotFound
  components/   BreathingCircle, ChatMessage, Mood* components, Navigation, ui/ (shadcn)
  hooks/        useChat (scripted replies), useMoodStorage (localStorage), useMoodSounds (Web Audio)
  lib/          utils
```

Routes: `/`, `/dashboard`, `/chat`, `/calm`.

## Known limitations

- Chat is rule-based and keyword-matched, not AI; replies can be repetitive or inappropriate to what the user wrote.
- No crisis detection beyond a keyword that triggers a breathing suggestion, and no helpline or emergency information is shown.
- Data lives only in one browser's `localStorage`: no sync, no backup, no export, and clearing site data erases it.
- Chat history is not persisted.
- No automated tests are present.
- `index.html` still has placeholder "Lovable App" title and description metadata, and the `package.json` name is still the scaffold default (`vite_react_shadcn_ts`).
- No deployment is configured or linked.

## Credits

Created by **AJ Abhishek** and **aman1011019** (the repository's two commit authors) as a hackathon project. Built with a Lovable scaffold, shadcn/ui, and Radix UI.
