<div align="center">

# 🧩 Pattern Console

**Offline-first study platform that takes you from first principles to interview- and build-ready across three tracks — DSA pattern recognition, system design, and AI engineering — shipped as a single static file with a serverless backend.**

[![Live Demo](https://img.shields.io/badge/demo-live-brightgreen)](https://prajwalkamble.github.io/DSA-SD-Prep/)
![Build](https://img.shields.io/badge/build-none%20(zero%20dependencies)-blue)
![Tracks](https://img.shields.io/badge/tracks-DSA%20%C2%B7%20System%20Design%20%C2%B7%20AI-8b7ff0)
![Frontend](https://img.shields.io/badge/frontend-HTML%20%7C%20CSS%20%7C%20JS-orange)
![Backend](https://img.shields.io/badge/backend-Supabase-3ecf8e)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

[Live demo](https://prajwalkamble.github.io/DSA-SD-Prep/) · [Architecture](#architecture) · [Engineering decisions](#engineering-decisions) · [Deployment](#deployment)

</div>

---

## What it is

Pattern Console is a single **`index.html`** — no framework, no build step — that runs **offline after first sign-in** and syncs to Postgres when online. It began as a trainer for *recognising the ~22 patterns* behind DSA problems and grew into a full self-study platform spanning three tracks, all taught with one method: **deliberate practice with spaced review.**

- **DSA** — pattern recognition (22 patterns), a 25-topic foundations track, and dynamic programming.
- **System Design** — core building blocks and guided case studies.
- **AI Engineering** — a **7-phase, 31-lesson** curriculum from the underlying math, through training your own models, to generative and agentic systems in production.

Reading, theory, and code live in the app; the heavy hands-on work — the code console's Python, or the AI track's model training — runs in the browser or in Colab/PyTorch, and every AI lesson ends with a concrete build task.

> **Live:** <https://prajwalkamble.github.io/DSA-SD-Prep/>

---

## Highlights

- 🧩 **Single-file SPA** — HTML + CSS + vanilla JS, zero dependencies, instant cache, no bundler; ~620 KB of app *and* curriculum in one file.
- 🎓 **Three curricula, one method** — DSA patterns, system design, and a full AI-engineering course (math → deep learning → transformers → GenAI → agents → production), each run through the same struggle-first, spaced-review loop.
- 📴 **Offline-first sync** — `localStorage` is the source of truth; Postgres is an idempotent sync target with union-merge reconciliation across devices.
- ⚙️ **In-browser code console** — custom test cases and a deterministic operation-counter that estimates complexity from a real run; optional Python (Pyodide) and optional AI review.
- 🔐 **Full auth flow** — email + password, username saved to the account, an in-app **6-digit email verification** step, and a developer-style boot screen on sign-in; RLS-scoped, secret-free client.
- 📊 **Progress that can't be gamed** — XP *derived* from completed work, levels, streaks, 16 badges, an editable goal countdown, and a shared leaderboard.

---

## Architecture

```
        Browser (GitHub Pages)                   Managed backend (Supabase)
  ┌──────────────────────────────┐        ┌─────────────────────────────────────┐
  │          index.html           │        │  Auth · Postgres · Row-Level Security│
  │  Home → Auth gate → 3 tracks  │        │                                     │
  │  (curriculum embedded as data)│ async  │  pc_state              (per-user)    │
  │                               │ upsert │  pc_leaderboard        (write-own)   │
  │  localStorage (source of      │───────►│  pc_leaderboard_public (read view)   │
  │  truth)                       │        │  username → auth user_metadata       │
  └───────────────┬──────────────┘        │  all RLS-scoped to auth.uid()        │
                  │ opt-in                 └─────────────────────────────────────┘
                  ▼
    Anthropic API (AI code review)  ·  Pyodide CDN (in-browser Python)
```

**Invariants:** every mutation writes to `localStorage` first; cloud sync is best-effort and idempotent (keyed by user); multi-device sync uses union-merge so progress is never lost (trade-off: deletions don't propagate); XP is *derived* from completed work, never stored; all curriculum is embedded as data, so there is no content backend to operate.

---

## Engineering decisions

| Decision | Why | Trade-off accepted |
|----------|-----|--------------------|
| Single static file, no build | Max reach (shared PCs, low-end phones); instant load; trivial hosting. | Larger single payload; manual code organisation over module tooling. |
| Curriculum embedded as data | Zero content infrastructure; works fully offline; the whole course is versioned in git. | Content edits require a redeploy — no live CMS. |
| Local-first state, async sync | Studying must never block on the network. | Eventual consistency; union-merge means deletions don't propagate. |
| Username in `auth.user_metadata` | One fewer table, trigger, and policy to operate; travels with the account. | Not a queryable column; no DB-level uniqueness. |
| Publishable key only in client | A public static site cannot safely hold private secrets. | All access enforced server-side via Row-Level Security. |
| Derived XP, not stored | Metrics can't be gamed or drift from reality. | Recomputed from source on every change. |
| Home open, resources gated | A public landing page; all progress lives behind an account. | Course content needs sign-in even for a quick look. |

---

## Tech stack

**Frontend:** HTML5 · CSS3 (responsive, dark/light) · vanilla JS (single file, no build)
**Visualisation:** hand-rolled SVG/Canvas (algorithm visualiser, 20+ modes; progress charts)
**Execution:** in-browser JS test runner + deterministic op-counter; optional Pyodide (Python)
**AI:** optional code review via the Anthropic Messages API
**Backend:** Supabase — Postgres, Auth (email + password, OTP verification), Row-Level Security
**Hosting:** GitHub Pages (static) · Supabase (managed backend)

The **AI Engineering track** *points to* Colab, PyTorch, and Hugging Face as the learner's hands-on environment — these are referenced by the lessons, not bundled into the app.

**Data model** (all RLS-scoped to `auth.uid() = user_id`): `pc_state` (per-user progress), `pc_leaderboard` (write-own) exposed publicly only through the `pc_leaderboard_public` view — display columns (name + scores), no `user_id`. Usernames are stored in `auth.users.raw_user_meta_data`.

---

## Feature reference

<details>
<summary><strong>DSA — patterns, foundations & DP</strong></summary>

- **22-pattern catalogue** — each with triggers, complexity, and a "learn it" flow
- **Foundations** — 25 topics across complexity/recursion, data structures, searching, and sorting, with a from-scratch console button
- **Dynamic programming** module and a **ten-stage guided Learning Path** from zero to interview-ready
- Quiz engine, curated problem sheets (Blind 75 / NeetCode 150), streaks, and 16 badges
</details>

<details>
<summary><strong>System Design</strong></summary>

- Core building blocks and guided case studies, tracked and XP-scored like the rest
- Studied topics feed the same progress, streak, and leaderboard systems
</details>

<details>
<summary><strong>AI Engineering — 7 phases, 31 lessons</strong></summary>

A structured route, taught in order, each lesson in *what / how / when* form with Python, a hands-on Colab task, and the single best resource:

1. **Foundations** — NumPy & vectorization, linear algebra, gradients & the chain rule, probability & softmax
2. **Classical ML** — the model/loss/optimizer frame, regression from scratch, overfitting & splits, trees/boosting, clustering & PCA
3. **Deep Learning** — neurons & MLPs, backpropagation, training dynamics, PyTorch, CNNs & embeddings
4. **Transformers & LLMs** — tokenization, attention & the transformer block, **build a tiny GPT**, pretraining→SFT→RLHF, running open models
5. **GenAI Engineering** — grounded prompting, embeddings & vector DBs, **RAG**, LoRA/QLoRA fine-tuning, evaluation
6. **Agentic AI** — tool use/function calling, ReAct loops & memory, LangGraph & multi-agent, guardrails & MCP
7. **Production & Integration** — serving/latency/cost, LLMOps & evals-in-CI, and a **Spring Boot + pgvector integration capstone**

The in-app text is the map and the theory; the four capstones (tiny GPT, RAG app, fine-tune, backend integration) are built in Colab/PyTorch and are the intended portfolio output.
</details>

<details>
<summary><strong>Practice tools</strong></summary>

- **Code console** — multi-language templates, syntax highlighting, bracket matching, autocomplete, custom test cases, and a deterministic complexity estimator; run history with diffs and editorials
- **Optional Python** via Pyodide and **optional AI code review** (where an Anthropic endpoint is available)
- **Algorithm visualiser** (20+ modes), **Problem of the Day**, **spaced-repetition review**, and **timed mock interviews**
</details>

<details>
<summary><strong>Platform & auth</strong></summary>

- Email + password accounts with a **username stored on the account**; **in-app 6-digit email verification**; a developer-style boot screen on sign-in
- Offline-first sync with a live status indicator; JSON backup / restore; shared leaderboard
- **Editable target date** with a live countdown; auth-gated course with **destination memory** (sign in and land on the page you clicked)
</details>

---

## Getting started

No install — it's a hosted web app.

1. Open [`prajwalkamble.github.io/pattern-console`](https://prajwalkamble.github.io/DSA-SD-Prep/) in any modern browser (desktop or mobile).
2. **Create an account** — username + email + password. If email confirmation is enabled, you'll get a **6-digit code** to enter in the app; otherwise you're signed straight in.
3. Pick a track — **Learning Path** (DSA), **System Design**, or **AI Engineering** — and work top-to-bottom. Progress saves automatically.
4. After first sign-in on a device, the course works **offline** and re-syncs on reconnect.

---

## Deployment

**Static site (GitHub Pages)**
```bash
git add . && git commit -m "Deploy Pattern Console" && git push origin main
# GitHub → Settings → Pages → Source: "Deploy from a branch" → main / root
```
Serves `index.html`. The embedded Supabase **publishable** key is public by design (RLS-enforced) and safe to commit.

**Backend (Supabase)** — auth, progress sync, and the leaderboard:
1. Create a project; copy the **Project URL** and **publishable (anon) key** into `index.html`.
2. Run the setup SQL in the SQL Editor to create the RLS-protected tables and the `pc_leaderboard_public` view.
3. **Choose an email-verification mode:**
   - *Frictionless:* **disable "Confirm email"** (Authentication → Sign In / Providers → Email) — sign-up logs users straight in.
   - *6-digit code:* keep confirmation **on**, configure **custom SMTP** (e.g. Gmail or Brevo), then edit the **Confirm signup** template to include `{{ .Token }}`. (Supabase locks template editing until custom SMTP is set.)

Usernames are written to `auth.users` metadata automatically at sign-up — no extra table required.

---

## Security model

- Passwords hashed by Supabase Auth — never stored in plaintext.
- Row-Level Security enforces `auth.uid() = user_id` on every table. The leaderboard is read through a view exposing only display columns (name + scores, no `user_id`), and the base table is not publicly readable.
- The **publishable** key is safe to ship — it grants nothing beyond what RLS permits at the database layer. The **secret** key never touches the client.
- Usernames live in `auth.users.raw_user_meta_data` (account-scoped), not in a public table.
- User code runs only in the browser's own sandbox (JS test runner / Pyodide) — no server-side execution and no external account access.

---

## Known limitations

- Account creation and sign-in need **one** online session (Supabase Auth); the course is offline thereafter until you sign out.
- The **6-digit code** flow requires custom SMTP plus a `{{ .Token }}` email template; without them, either use the default confirmation link or disable confirmation entirely.
- Multi-device sync uses union-merge, so a reset on one device **won't** clear another.
- **Python execution** requires loading Pyodide (online), and **AI review** requires a reachable Anthropic endpoint — both fall back to the offline estimator and test runner when unavailable.
- The **AI Engineering track** teaches concepts, code, and tasks in-app, but the actual model-building runs in Colab/PyTorch — a browser file cannot train models on a GPU.
- Course resources are gated behind sign-in by design; only the landing page is open.

---

## Disclaimer

Pattern Console is an **educational tool** for interview preparation and self-study. It is not affiliated with LeetCode, NeetCode, Hugging Face, or any other platform; external problems and resources link to their original sources.

---

<div align="center"><sub><em>Designed and built end-to-end — frontend, curriculum, data model, and serverless backend.</em></sub></div>
