# answer-me · Claude skill

> Ultra-compressed responses for Claude. **Same technical substance, ~75% fewer tokens** — and answers you can read and act on in seconds, instead of wading through long, unformatted walls of text.

---

## What it does

Switches Claude into a terse, high-signal voice: articles, filler, and hedging dropped — technical facts, code, and error messages kept exact. Four intensity levels from lightly trimmed to maximally compressed, plus a **custom lens** that angles the answer to whatever you're using Claude for.

**Why it helps:**

- 💸 **Fewer tokens** — responses shrink ~75%, so you spend less and get answers faster.
- ⚡ **Quick to act on** — the fix or the verdict is up front; no scrolling to find it.
- 👀 **Easy to scan** — short lines and arrows (X → Y), not dense paragraphs.
- 😌 **Less frustration** — no reading and re-analyzing filler and hedging just to reach the point.

---

## Trigger phrases

| Turn it on | Switch level | Turn it off |
|---|---|---|
| `/answer-me` · `answer me mode` · `use answer-me` · `less tokens` · `be brief` | `/answer-me lite\|full\|simple\|ultra` · `/answer-me custom <lens>` | `stop answer-me` · `normal mode` |

---

## Intensity levels

Default is **`full`**.

| Level | What you get |
|---|---|
| `lite` | No filler or hedging. Full sentences kept. Professional but concise. |
| `full` | Articles dropped, fragments OK, short synonyms. Maximum signal, minimum words. |
| `simple` | Heavy compression in plain English for beginners. Short sentences, arrows (X → Y), no obscure abbreviations. |
| `ultra` | Maximum compression. Abbreviations (DB / auth / fn), causal arrows, one word where one word works. |
| `custom <lens>` | Angle the answer to **what you're using Claude for** — a role or a use-case. Matches its vocabulary, priorities, and framing; terse core stays. Try `custom founder`, `custom senior-SRE`, `custom security-review`, `custom teaching`. |

---

## Examples

The **Traditional** row is a normal default answer. Every level below says the same thing with less — no facts lost.

### 1. "My search box calls the API on every keystroke and the app lags"

*≈ 85 words → 22 at `full`. Same fix, a quarter of the reading.*

| Mode | Response |
|---|---|
| **Traditional** | Ah, this is a classic search-performance issue. Your input's `onChange` handler fires a network request on every single keystroke, so a ten-character query means ten API calls in quick succession — that floods the backend and makes the UI feel sluggish. The standard fix is to debounce the input: wait until the user pauses typing (around 300 ms) before sending the request. You can do it with a `setTimeout` you clear on each keystroke, or a `debounce` helper from a library like lodash. |
| `lite` | Your search box fires an API call on every keystroke, which floods the backend and lags the UI. Debounce the input so it only calls after the user pauses typing (~300 ms). |
| `full` | Every keystroke fires a request → backend flooded, UI lags. Debounce input: call ~300 ms after last keystroke. `clearTimeout` each change, or lodash `debounce`. |
| `simple` | Each keystroke → one API call → backend flooded → lag. Debounce: wait ~300 ms after typing stops, then call. |
| `ultra` | Keystroke → req each time → flood → lag. Debounce ~300 ms. |

### 2. "Should I store JWTs in localStorage or an httpOnly cookie?"

*≈ 95 words → 13 at `ultra`. Same verdict, every trade-off intact.*

| Mode | Response |
|---|---|
| **Traditional** | Great security question — both are common and each has trade-offs. Storing the token in `localStorage` is convenient because your JavaScript can read it and attach it to requests, but it's fully exposed to cross-site scripting: if an attacker runs JS on your page, they can steal it. An `httpOnly` cookie can't be read by JavaScript at all, so it's safe from XSS, but it introduces cross-site request forgery risk, so you'd need CSRF protection such as a `SameSite` attribute or an anti-CSRF token. For most web apps, the `httpOnly` cookie is the more secure default. |
| `lite` | `localStorage` is easy for JavaScript to read but exposed to XSS token theft. An `httpOnly` cookie is safe from XSS but needs CSRF protection. For most web apps, prefer the `httpOnly` cookie. |
| `full` | `localStorage` = easy JS access, but any XSS reads it → token stolen. `httpOnly` cookie = JS can't read it (XSS-safe), but opens CSRF → add `SameSite` or anti-CSRF token. Default `httpOnly` cookie. |
| `simple` | `localStorage` → JS can read → XSS steals token. `httpOnly` cookie → JS can't read → XSS-safe, but add CSRF protection. Prefer the cookie. |
| `ultra` | `localStorage` → XSS steals it. `httpOnly` cookie → XSS-safe, needs CSRF guard. Use cookie. |

### 3. Custom lens — same question, angled to what you're using Claude for

`/answer-me custom <lens>` keeps the terse core but re-frames the answer for your role or use-case, so it comes back relevant to *your* goal — not generic.

**Example A — "Should we add a Redis cache?"**

| Lens | Response |
|---|---|
| `custom founder` | Cache = faster app, happier users, but one more thing to run and pay for. Add it when speed actually costs you signups. Not there yet? Ship without. |
| `custom senior-SRE` | Cache only if read-heavy + measured latency pain. Risks: invalidation, cold-start stampede, extra failure mode. Add TTL + jitter. No cache for write-heavy or low-QPS. |
| `custom security-review` | Cache = new data store → new attack surface. Don't cache PII/secrets unencrypted. Lock Redis auth + network. Short TTLs limit stale-secret exposure. |

**Example B — "Our checkout API is slow — where do I start?"**

| Lens | Response |
|---|---|
| `custom junior-dev` | Time the request end-to-end, find the slowest step — usually a DB query or an external call. Add logging around each stage, then fix the biggest one first. |
| `custom senior-SRE` | Look at p95/p99 traces, not averages. Suspect N+1 queries, a missing index, or a sync external call in the hot path. Add a span per stage, cache or make the slow dependency async, set timeouts. |
| `custom product-manager` | Slow checkout = lost sales. Get current p95 vs target, ship the single fix with the biggest drop first, measure conversion after. Don't block the release on a rewrite. |

---

## Auto-clarity

Compression pauses automatically — and resumes right after — for:

- Security warnings
- Irreversible or destructive action confirmations
- Multi-step sequences where fragment order could mislead
- Moments where the user is clearly confused

---

## Boundaries

- **Code, commits, and PRs** are always written normally, at every level.
- **Level persists** for the session until you change it.
- **`stop answer-me`** or **`normal mode`** exits entirely.

---

## Install

Copy `SKILL.md` into `~/.claude/skills/answer-me/`, run `/reload-skills`, then invoke with `/answer-me`.
