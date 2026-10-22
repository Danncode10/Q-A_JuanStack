# JuanStack: Technical Q&A (plain-language version)

> **For:** PSC XI Regional, Oct 23, 2026. Q&A is 10 minutes, and **all three members must answer**. "Technology Viability" is 20% of the score, and "Clarity" (direct, precise answers) is 40% of the live score.
> **How to use this file:** Part 1 explains how the system works in everyday words, so *you* understand it. Part 2 is the exact lines to say. Read Part 1 once, then drill Part 2 out loud.
> Every fact here was checked against the code on Oct 22, 2026.

---

# Part 1. Understand it first (for you, not for the judges)

You don't need to read code to explain this. Think of JuanStack as a **condo building**.

| The real thing | The condo version |
|---|---|
| **Supabase database** | The building. Every business's records live here. |
| **Organization** | One unit. Each business has its own unit. |
| **Login (Supabase Auth)** | The ID check at the lobby. No ID, no entry. |
| **RLS (Row-Level Security)** | The keycard lock **on every unit door**. Even if you get past the lobby, your card only opens your own unit. The lock is built into the door (the database), not into the guard (our app). So even if our app had a mistake, the door still won't open. |
| **Roles (owner, admin, staff)** | Inside the unit, some people can open the safe, some can't. |
| **Rate limiting (Upstash Redis)** | A turnstile that only lets so many people through per minute, so no one can flood the building. |
| **Service layer** | The building's front desk. All requests for records go through one desk with fixed rules, instead of everyone wandering into storage rooms. |
| **Jev** | The receptionist who reads your request and points you to the right department. Cheap, fast, only reads your latest message. |
| **Gemini (the LLM)** | The staff member who actually writes you a reply. Only gets the one tool your request needs. |
| **BIR tax engine** | The accountant with a calculator and a rulebook. The AI never touches the calculator. |

**The one sentence to remember:** *"Login checks who you are, RLS checks what you're allowed to see, and the database enforces it, not just the app."*

### How a chat message travels

1. You type "What's due this month?"
2. **Turnstile:** Redis checks you haven't sent too many messages.
3. **Lobby ID:** the server confirms you're logged in.
4. **Allowance:** the database checks you still have AI budget this week.
5. **Receptionist (Jev):** sorts it: "this is about BIR deadlines."
6. **Staff (Gemini):** writes the answer, using only the deadlines tool.
7. The deadlines tool asks the database, and **RLS** only returns *your* business's data.

### The words judges might use

- **Architecture:** how the parts are arranged and talk to each other.
- **Serverless:** we don't run our own servers. Vercel and Supabase run them and add more when traffic goes up.
- **Monolith vs. microservices:** monolith = one app with many parts inside. Microservices = many small separate apps talking over the network. **We are a monolith** (a well-organized one). Don't say microservices.
- **JWT:** the digital ID card you get after logging in. It's signed, so it can't be faked.
- **Service-role key:** the master key that opens every door. Only our server has it, never the browser.
- **Fail closed:** if the turnstile breaks, it locks instead of letting everyone in.
- **Encryption in transit / at rest:** scrambled while traveling (HTTPS) / scrambled while stored on disk.
- **SQL injection:** an attack where someone types database commands into a form. We're protected because we never glue user text into database commands.
- **Prompt injection:** tricking an AI with sneaky instructions. Our AI has almost no power, so there's little to trick it into.

---

# Part 2. What to say

**Golden rule:** answer in **one or two sentences**, then stop. The quoted lines are examples, not scripts: keep the idea, say it your way (see `00_README.md`, "How a top team answers"). Let them ask more. Short and sure beats long and shaky.

## A. "Did you build this with AI?" (likely question, be ready)

**Say:**
> "Yes, we use AI coding tools, like most startups now. What we designed ourselves is the architecture and the rules: the security model, the data separation between businesses, and the tests. The AI writes code inside those rules, and automated tests check every change before it goes live."

*If they push "do you understand your own code?"*
> "We understand the system: how data flows, where it's protected and why. For example, I can walk you through how one business is blocked from seeing another's data." (Then give answer C1.)

*If they push "how do you know the AI-written code is correct?"*
> "Tests. The project has 58 automated test files, including tests for the BIR tax engine and a test that blocks any industry pack from reaching into the database. Every change must pass them before it goes live."

**Why this works:** it's honest, and it shifts to what judges actually score: does the system work and is it safe. Never say "I just prompted it."

## B. Architecture

**B1. "Explain your architecture."**
> "It's a web app on Vercel with a Supabase Postgres database. The app has three layers: the screens, a service layer where all the business rules live, and the database, which enforces security on its own. The AI chat sits on top, and the tax computations run in fixed code, not in the AI."

**B2. "Is it microservices?"**
> "No, it's a modular monolith: one codebase, organized into clear modules. For a small team that's faster and has fewer things that can break. Background jobs, like BIR deadline reminders, run separately on Supabase scheduled functions. If one part needs to scale on its own later, we can split it out."

**B3. "How do industry packs work? Separate apps?"**
> "One app. AttyJuan, JuanConstruction and JuanProperty are plug-in modules inside it. Each pack has its own folder and can only use a small approved set of core features. We have automated tests that block a release if a pack tries to reach into the database or another pack directly."

**B4. "Can it scale? What if you get thousands of users?"**
> "The app side scales automatically: Vercel adds more copies of our app as traffic grows. The database is the part we scale on purpose: we upgrade Supabase's server size as users grow, and later add read replicas and caching. We already have the basics in place: database indexes, connection pooling, and rate limits that protect the system from spikes."

*If they push "does it auto-scale?"*
> "The app layer, yes. The database, no. On Supabase you choose the server size, and upgrading takes a click with a short restart. That's normal for Postgres, and it's cheaper than paying for a huge database before we need it."

*If they push "what's your plan when you have many users?"* Give the stages:

| Stage | Users (rough) | What we do |
|---|---|---|
| Now (beta) | up to ~1,000 | Free/Pro tier. Indexes on key columns, connection pooling, rate limits. |
| Growth | ~1,000 to 10,000 | Upgrade Supabase compute size. Cache data that rarely changes (BIR deadlines, tax tables) instead of reading the database every time. |
| Scale | 10,000+ | **Read replicas** (copies of the database only for reading, e.g. reports and dashboards), so the main database handles writes. Move heavy jobs like reports and AI Secretary scans to background queues. |

*If they ask "what will break first?"*
> "The database, because every request touches it. That's why we watch database load and upgrade it first. The AI cost is already capped per user, so more users can't run up an unlimited bill."

### Understand it (for you)

- **Vercel = auto.** Like a fast-food chain that opens more counters when the line gets long. You don't do anything.
- **Supabase database = manual.** One big kitchen. When it's too busy, you **move to a bigger kitchen** (bigger compute). That's called **vertical scaling**.
- **Read replica** = a **photocopy of the database** that's only for reading. Most of what users do is *look* at things (dashboards, invoices), so you send that to the copy and let the main one handle saving.
- **Caching** = **remembering an answer** so you don't ask the database again. BIR deadlines don't change every second, so you remember them.
- **Connection pooling** = instead of every visitor opening their own phone line to the database, they **share a few lines**. Supabase includes this (Supavisor).
- **Indexes** = the **table of contents** of the database, so it finds rows fast instead of reading everything. We have 25 indexes.
- **Horizontal vs. vertical:** horizontal = more machines (Vercel does this). Vertical = a bigger machine (how we grow the database).

⚠️ Don't say "everything auto-scales." The database doesn't, and an IT judge knows that.

**B5. "What's your tech stack?"**
> "Next.js and React for the web app, Supabase for the database and login, Upstash Redis for rate limiting, and OpenRouter to reach the AI models: a small model called Jev to sort messages and Google's Gemini Flash-Lite to answer."

## C. Data security (Supabase)

**C1. "How do you keep one business's data away from another?"**
> "Every business is its own organization in the database, and we use Postgres Row-Level Security. The database itself checks every request: you can only see rows that belong to your organization. So even if our app code had a bug, the database still wouldn't hand over another business's data."

*If they push:* "RLS is on for every table, 36 tables and 180 security rules. Users join an organization with a role, owner, admin or staff, and some actions need the higher roles."

**C2. "Your database key is visible in the browser. Isn't that a risk?"**
> "That's the public key, and it can't do anything alone. Every request still needs a valid login and passes Row-Level Security. The master key that bypasses security stays on our server only."

**C3. "Can a user make themselves admin?"**
> "No. The database has triggers that block anyone from changing their own role or removing the business owner, even if they call the database directly."

**C4. "Do you store passwords? How is login done?"**
> "Users can sign in two ways: with Google, or with email and password. With Google, we never see a password at all, Google confirms who they are. With email, Supabase Auth handles it and stores the password only as a scrambled hash, never as plain text. Either way, the user gets a signed login token, and sign-in is rate-limited by email and by IP to stop password guessing."

*If they ask about Google Calendar:*
> "Connecting Google Calendar is separate from login and optional. Google gives us a token to read the calendar, and we encrypt that token with AES-256 before saving it. Only members of that business can use it, and disconnecting deletes the imported events."

*For you:* signing in with Google is called **OAuth**: Google vouches for you, like showing a company ID instead of making a new one. A **hash** is a one-way scramble: you can check a password against it, but you can't turn it back into the password.

**C5. "Is the data encrypted?"**
> "Yes, in transit with HTTPS, and at rest on Supabase's storage."
> ⚠️ Never say "end-to-end encrypted." It isn't, because the server needs to read the numbers to compute taxes.

**C6. "What about SQL injection?"**
> "We never build database commands by gluing in what the user typed. Everything goes through Supabase's query builder, which sends the user's text as a separate value, so it's treated as data, never as a command. Our search boxes also strip special characters first. And even if something slipped through, Row-Level Security still limits the query to that user's own business."

*If they push "how exactly?":*
> "It's parameterized queries. The command and the values travel separately, like a form with fixed blanks. Whatever you type goes in a blank; it can't change the form."

*For you:* SQL injection is when someone types a database command into a form (e.g. `' OR 1=1 --` in a login box) and a careless app pastes it straight into its command, so the database runs it. We're protected in three layers: (1) the Supabase query builder keeps the user's text separate from the command, (2) search boxes remove characters like `% , ( ) *` before searching (`cleanSearch` in `src/services/clients.ts`), (3) RLS: even a bad query can only reach the user's own organization's rows.

**C7. "How does a client sign a 2307 without an account?"**
> "We send them a private link. The link's secret code is stored only in scrambled (hashed) form, it expires, and they must type the correct TIN before they see anything. Wrong TINs use up a limited number of tries, and the link is rate-limited."

## D. The AI (Jev and the LLM)

**D1. "How does your AI work?"**
> "It's a decision tree. A small, very cheap model called Jev reads each message and answers two questions in one call: what's it about, and what exactly do you want. If it's a common request, like 'how much tax do I owe' or 'what's on my schedule tomorrow,' our own code answers it directly, no big model. If details are missing, Juan asks for them first. If it's off-topic, it's blocked. Only open questions go to Gemini, and Gemini only gets the tools for that topic."

*For you:* the tree is a real data structure in the code (`JUAN_TREE` in `src/ai/juan-tree.ts`). Each node has a test and either child nodes or a reply. The code starts at the root and goes into the first child whose test passes, until it reaches a reply. Picture: `../JuanStack Info/Files/AI_Cost_Juan_Tree.png`.

**D2. "Why two models instead of one?"**
> "Cost. Jev costs $0.042 per million tokens and its output is free, so sorting a 30-word message costs ₱0.0018. When our own code can answer, that's the whole cost, 84% less than a full chat. Only open questions go to Gemini, and a full chat is about one centavo. A normal Solo user costs us about ₱2.35 a month in AI."

*If they push "is it faster?":* "For tree answers, yes: there's no big model to wait for. But most of our response time today is the server checking login, limits and the database, and that's what we're optimizing next." (Honest: on the live site both paths took a few seconds; don't promise a number.)

**D3. "What if the AI gets the taxes wrong?"**
> "The AI never computes taxes. Tax math runs in fixed, rule-based code built from BIR rules. The AI only explains and guides. AI talks, the calculator calculates."

**D4. "Can someone trick the AI into leaking data?"**
> "There's very little it could leak. The AI only acts as the logged-in user, so the database security still applies. Its tools can read the user's own schedule and prepare items the user has to confirm. It can't send emails, change tax data, or file anything."

**D5. "What data goes to the AI company?"**
> "Only the chat text and schedule titles. Invoices, TINs, bank data and tax computations never go to the AI."

**D6. "What if the AI provider goes down?"**
> "If the sorting model is slow, we fall back to a safe default after 4 seconds. And because we reach the models through one gateway, OpenRouter, we can switch providers without changing the app. Everything else in JuanStack, invoices, schedule, forms, works without the AI."

## E. Abuse and cost control

**E1. "What stops someone from spamming your AI and running up your bill?"**
> "Three layers. A rate limit of 10 messages per minute per user and 30 per IP. A daily cap of 100 messages. And a weekly AI budget per user: we record what each message actually costs, and Juan pauses when the allowance is used, while the rest of the app keeps working."

**E2. "What if your rate limiter goes down?"**
> "Sensitive actions like login, sign-up and chat fail closed: they refuse requests instead of running with no limit."

## F. BIR filing

See **[02_BIR_Filing_QA.md](02_BIR_Filing_QA.md)**. Short version:
> "Not yet. Filing is turned off in our beta on purpose. To submit directly to BIR, software must be certified by BIR as a Tax Software Provider, the same way Taxumo and JuanTax are. Today we prepare the return so it's ready to file, and certification is on our roadmap."

---

# Part 3. Don't say

| Don't say | Say instead |
|---|---|
| "We use microservices." | "Modular monolith on serverless." |
| "It's end-to-end encrypted." | "Encrypted in transit and at rest, protected by login and Row-Level Security." |
| "It's 100% secure / unhackable." | "We have several layers: rate limits, login, database security rules." |
| "The AI computes your taxes." | "Fixed code computes; the AI explains." |
| "We can file to BIR." | "We prepare the return; BIR certification is next." |
| "I just prompted the AI." | "We designed the architecture and rules; AI tools help write the code." |
| "I don't know." (and stop) | "Good question. I'd want to confirm that before giving you a number. What I can say is…" |

---

# Part 4. Private prep (not for the judges)

Things a very sharp IT judge *could* poke at. Don't bring them up. If asked, the answer is ready.

1. **Full Content-Security-Policy isn't set yet** (a browser rule that limits which scripts can run). If asked about XSS: "React escapes user text by default, and a full CSP is on our hardening list."
2. **Background AI Secretary job reads whole tables** with the master key. If asked: "It's a prototype job; before scale it will only read recently changed rows."
3. **Not yet checked:** the Supabase region (Singapore is ideal), backup tier, and that the "Business A can't see Business B" test passes. If you have 10 minutes tonight, do the test.
4. `../JuanStack Info/Data_Privacy_QA.md` says the chat limit is "5 per 10 seconds." The real numbers are **10 per minute per user, 30 per minute per IP.** Use the new numbers.
5. **Concept note vs. the real code** (checked Oct 22). Judges have the concept note, so they may ask about these:
   - **Hosting:** the concept note's expense table says **Netlify**, but juanstack.com actually runs on **Vercel** (Singapore). If asked: "We're on Vercel." Fix the note before Clusters.
   - **Receipt scanning:** the concept note says Juan scans receipts, but there's **no scanning code yet**. If asked: "Receipt scanning is in development; it pre-fills an expense that the owner confirms."
   - **SLSP file:** the concept note lists SLSP, but only **SAWT and QAP** exist in the code. Say "SAWT and QAP today, SLSP next."
   - **Forms:** 2307, 2551Q, 2550Q, 1701Q and 1601-EQ all appear in the BIR engine. ✅
6. **Split the Q&A across the three of you.** Suggestion: one person takes Architecture + Security (B, C), one takes AI + cost (D, E), one takes BIR + business. All three must answer something.
