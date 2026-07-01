# Pattern Console

**A single-file training environment for learning to *recognise* the patterns behind data-structures & algorithms interview problems — not just to solve them.**

Most interview prep optimises for the wrong number: problems completed. Pattern Console optimises for the one that actually transfers — how quickly you recognise *which* of the ~22 recurring patterns a new problem reduces to, and why. It packages a structured curriculum, a live code sandbox, spaced review, and deliberate-practice guardrails into one page that runs anywhere, online or off.

---

## Why it exists

Technical interviews are, in practice, a pattern-recognition test wearing a problem-solving costume. The same two dozen shapes — sliding window, two pointers, monotonic stack, BFS/DFS, binary search on the answer, 1-D and 2-D dynamic programming — reappear behind hundreds of superficially different questions. A candidate who has memorised 300 solutions but can't map an unseen prompt to its underlying pattern will stall. A candidate who recognises the shape in the first two minutes rarely does.

Grinding a random problem list doesn't reliably build that skill. It produces the *feeling* of progress — a rising solved count — without the *transfer*: fluency on problems you haven't seen. The gap is methodological, not motivational. Most people skip the struggle, read the solution too early, never re-derive it cold, and never say out loud *why* the pattern fits.

Pattern Console is built to close that gap — to make the pattern the unit of study and deliberate practice the default path, rather than something you have to remember to do.

## What it is

A self-contained web app — a single HTML file, no build step, no server required — organised around three jobs:

**Learn.** A pattern catalogue (~22 patterns), a foundations track covering complexity analysis and the core data structures, and dedicated dynamic-programming and system-design modules. A ten-stage guided **Learning Path** sequences all of it from zero to interview-ready, so there is always an obvious next step.

**Practice.** A code console with multi-language templates, syntax highlighting, custom test cases, and a deterministic operation-counter that estimates runtime complexity from an actual run — plus optional in-browser Python execution and optional AI code review. Alongside it: a quiz engine, curated problem sheets, an algorithm visualiser, a daily problem, a spaced-repetition queue, and timed mock-interview sessions.

**Track.** Progress that only ever accrues from work actually completed — XP, levels, streaks, and badges derived from real activity, never from busywork. Everything persists locally; an optional cloud layer syncs it across devices; a shared leaderboard shows where you stand.

## How it works

### The method

The tool encodes one practice loop and nudges you toward it on every problem:

1. **Struggle first** — 20–30 minutes, no hints, no AI. The friction is where the learning is.
2. **Editorial as a last resort**, never a first move — and only after a genuine attempt.
3. **Re-derive from a blank page** 24–48 hours later. If you can't reproduce it cold, you didn't learn it.
4. **Explain the *why* out loud** — which pattern, and how you'd have spotted it — plus time and space complexity and edge cases, on every problem.
5. **Review on a schedule.** A spaced-repetition queue resurfaces past problems before you forget them, so retention compounds instead of decaying.

This is deliberate practice applied to algorithms. The point isn't to finish problems; it's to build recognition that survives to the interview.

### The system

- **Single file, zero dependencies by default.** The entire application — logic, styles, and content — ships as one HTML file that runs from disk or any static host. It works fully offline; the only external calls are opt-in (Python runtime, AI review, cloud sync).
- **Local-first persistence.** All progress lives in the browser, so the tool is usable with no account and no network.
- **Optional cloud sync.** Signing in mirrors your state to a per-user record and pulls it back on any device — same account, any OS, any network. Access is enforced server-side with row-level security, so each account can only ever read its own data.
- **Metrics that can't be gamed.** XP is *derived* from completed work rather than stored and incremented, so the numbers can't drift out of sync with what you've actually done.

## When — the plan

The curriculum is designed to be *run*, not just browsed, against a target date you set. The app shows a live countdown to that date and builds a week-by-week plan from it. The default arc is nine weeks, ordered easy → hard so each week builds on the last:

| Week | Focus |
|:---:|:---|
| 1 | Foundations · Arrays & Hashing |
| 2 | Two Pointers · Sliding Window |
| 3 | Stack · Binary Search |
| 4 | Linked List · Trees |
| 5 | Tries · Heaps / Priority Queues |
| 6 | Backtracking · Graphs |
| 7 | Advanced Graphs · 1-D Dynamic Programming |
| 8 | 2-D Dynamic Programming · Greedy · Intervals |
| 9 | Bit Manipulation · Math · System Design · timed mocks |

**Daily cadence:** a fixed study block; a few problems run through the loop above; roughly 70% new material and 30% spaced review; one timed mock interview each week to rehearse under pressure. The streak exists to protect the habit — consistency across nine weeks beats intensity in any single session.

## Design principles

- **The pattern is the unit, not the problem.** Every feature exists to build recognition and transfer, not to inflate a completion count.
- **Deliberate practice is the default.** The path of least resistance is the correct study behaviour, not a shortcut around it.
- **Offline-first, account-optional.** The core experience must work on a plane with no login. The cloud is an enhancement, never a dependency.
- **Honest metrics.** A number reflects real work or it doesn't appear.
- **One file.** No build, no framework, no toolchain to rot — it will still open in a browser years from now.

## Scope & non-goals

This is a focused learning instrument, and it is deliberately **not**:

- a competitive-programming trainer — it targets interview patterns, not contest mathematics;
- an online judge — it runs your own tests and estimates complexity; it is not a verdict server;
- a content marketplace or a multi-tenant SaaS;
- a substitute for reading primary sources and doing the reps. It is the scaffolding around that work, not a replacement for it.

## Status

In active use as structured preparation toward a software-engineering role. Content and roadmap are stable; the application is maintained as a single file with a regression suite covering the core flows.
