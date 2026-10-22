# JuanStack Cheat Sheet (read this right before going on stage)

## The one sentence
> **"You run the business, Juan runs the back office."**
> JuanStack is an AI business assistant for Filipino small businesses: books, BIR forms, schedule, team and reports in one place, plus a system for their industry.

## The three pillars (use for "what's innovative?")
1. **An assistant, not software.** You ask in Taglish; Juan prepares it; you confirm.
2. **AI you can trust with money.** The AI talks; fixed BIR rules calculate. The AI never does tax math.
3. **Compliance as a byproduct.** Daily invoices and payments fill the BIR forms. No quarter-end scramble.

## Three stories (use for any "give an example")
1. **The lost 2307.** A lawyer bills a company, which withholds tax. If the client never sends the 2307, the lawyer can't claim the credit and **pays that tax twice**. In JuanStack, the client signs the 2307 online with the invoice.
2. **The ₱1,000 penalty.** One late BIR return costs at least **₱1,000** plus 10% surcharge and 6% interest. **₱299 a month is less than one penalty.**
3. **Nights on paperwork.** Our interviews with **3 law offices and 1 construction firm**: owners don't lose money from bad management, they lose it because **no one tracks the back office.**

## Numbers

| | |
|---|---|
| Price | **Free · Solo ₱299 · Team ₱999 (+₱89/extra user)** |
| ₱299 is | **under ₱10 a day** |
| Cost per Solo user | **~₱10/month** → we keep **₱273–₱289** |
| One AI chat | **₱0.0107** (Jev + Gemini) · our code answers **₱0.0018** (84% less) · fixed replies **₱0** |
| AI per Solo user | **₱2.35/month** normal (220 chats) · ₱32.04 most possible |
| Break-even | **15 Solo users** (registered) · 1 during pilot |
| Goal | **100 paying businesses in 12 months** (~9/month) |
| Revenue at 100 | **₱36,900/month** · net **~₱24k–27k/month** |
| Ask | **₱300,000**: 110k product · 50k CPA · 115k first 100 users · 25k registration + contingency |
| Market | **1.24M MSMEs** (DTI 2024) · **120,000+** lawyers, contractors, real estate |
| Lawyers | **101,368** (Supreme Court list) |
| Contractors | **19,168** PCAB-licensed |
| Laws | EOPT Act **RA 11976** · Data Privacy Act **RA 10173** · e-invoicing by **Dec 31, 2026** |
| SDGs | **8** (decent work, economic growth) · **9** (industry, innovation, infrastructure) |

## Technical in one breath
> "Next.js on Vercel, Supabase Postgres. Every business is isolated by **Row-Level Security** in the database itself. A **decision tree**: **Jev** sorts the message, our own code answers common requests, off-topic is blocked, and only open questions reach **Gemini**. **Rate limits + a weekly AI budget** per user. It's a **modular monolith** on managed cloud services."

## Honest "not yet" answers
| Asked | Say |
|---|---|
| Paying users? | "Not yet, on purpose: no charging for tax work until a CPA certifies the rules." |
| File to BIR? | "Filing is off in beta. Direct filing needs BIR certification as a Tax Software Provider, like Taxumo. That's on our roadmap." |
| Receipt scanning? | "In development." |
| AttyJuan ready? | "Its case management exists as our first prototype; we're bringing it into JuanStack." |
| Data Privacy compliant? | "NPC registration is in months 1–3. Each business is already isolated at the database level." |
| Mobile app? | "Web works on phones; iOS and Android are in development." |

## Never say
"We file to BIR." · "BIR-accredited." · "End-to-end encrypted." · "100% secure." · "Microservices." · "No competitors." · "The AI computes taxes." · "We're DPA compliant."

## Last line if you get the chance
> "Every Juan in business deserves an assistant. We built one."
