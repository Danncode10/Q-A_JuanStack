# Built vs. Planned (read this first)

> Checked against the code on Oct 22, 2026.
> **Why this matters:** judges forgive "not yet." They don't forgive a claim that falls apart on the next question. Being exact about what's built is what makes you sound like a real founder.

## How to talk about status
- ✅ **"Working today"**: you can show it.
- 🔨 **"In development"**: started or partly there.
- 📅 **"Planned / funded"**: in the 12-month plan.
- 🚫 **"Turned off on purpose"**: built but blocked until it's safe.

## The table

| Feature | Status | How to say it |
|---|---|---|
| Login (Google or email/password) | ✅ | "Working today." |
| Each business isolated in the database (RLS) | ✅ | "Working today: the database itself blocks other businesses." |
| Team roles: owner, admin, staff | ✅ | "Working today." |
| Invoices | ✅ | "Working today." |
| 2307 signing link for clients (TIN check, expiring link) | ✅ | "Working today." |
| Transaction log (books) | ✅ | "Working today." |
| BIR engine: 2307, 2551Q, 2550Q, 1701Q, 1601-EQ | ✅ in code | "Juan prepares them. A CPA certifies the rules before we charge." |
| SAWT and QAP attachment files | ✅ in code | "Prepared and checked against BIR's published validation rules." |
| SLSP attachment file | 🔨 | "SAWT and QAP today, SLSP next." |
| Approve and file to BIR | 🚫 | "Off on purpose until CPA certification and BIR software certification." |
| Juan AI chat, Taglish, confirmation cards | ✅ | "Working today." |
| Juan's decision tree (common requests answered with no Gemini, off-topic blocked, missing details asked) | ✅ live Oct 22 | "Working today." Share of messages it handles: not measured yet. |
| Sparkle button: explains BIR terms | ✅ | "Working today." |
| Schedule + BIR deadlines + Google Calendar sync | ✅ | "Working today." |
| Rate limits + weekly AI budget | ✅ | "Working today." |
| Receipt scanning (photo → expense) | 🔨 | "In development: it pre-fills an expense the owner confirms." |
| **AttyJuan** | 🔨 | The pack switch works: it changes the words (Cases, Hearings) and Juan's rules. **Case files and court calendar screens aren't inside JuanStack yet**; they exist in our separate AttyJuan prototype. Say: *"AttyJuan's case management exists as our first prototype, and we're bringing it into JuanStack."* |
| **JuanConstruction, JuanProperty** | 🔨 | Same: the switch and words work, screens are next. Say: *"The packs switch on today; their industry screens are our next build."* |
| iOS / Android apps | 🔨 | "The web works on phones; native apps are in development." |
| Online payments (PayMongo) | 📅 | "Launches with paid plans in months 4–6." |
| CPA certification | 📅 | "First use of the funding." |
| NPC / Data Privacy registration | 📅 | "Months 1–3." |
| BIR Tax Software Provider certification | 📅 | "On our roadmap after business registration." |
| Hosting | ✅ **Vercel** (check if it's the free Hobby plan) | Today: Vercel. Vercel's free plan is **non-commercial only**, so for paid launch: Netlify (as the concept note budgets) or Vercel Pro $20/mo. Say: *"Beta runs on Vercel's free tier; at paid launch we move to a plan that allows commercial use."* See file 10, section D. |

## The concept note says "Three are ready today"
The judges read this. If they ask to see AttyJuan's case files, don't say "it's ready." Say:
> "The AttyJuan pack is live as a setting: it changes JuanStack into a law office's language and rules. Its full case management was our first prototype, and we're moving it into JuanStack now."

## Before Clusters (Nov 10), if you win
Fix the concept note: say where you host today (Vercel) and the commercial-use plan for launch · receipt scanning → in development · SLSP → next · be specific about what each industry system has today.

> ⚠️ I only checked this repo. If the pack screens live in another branch or repo, update this table.
