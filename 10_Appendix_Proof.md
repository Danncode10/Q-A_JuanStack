# Appendix: Proof & Sources

> For when a judge asks **"where did you get that number?"** Every figure has its math and its source.
> Prices checked live on **Oct 22, 2026**. Exchange rate used everywhere: **₱58 = $1**.
> **Tip:** screenshot the ⭐ items tonight and put them in backup slides (offline, since the guidelines warn about internet).

---

## A. AI model prices

| Model | What it does for us | Input (per 1M tokens) | Output (per 1M tokens) | Source |
|---|---|---|---|---|
| **Jev 1.13** (`typesafe/jev-1.13`) by TypeSafe | Sorts each message into an intent | **$0.042** (₱2.44) | **$0, free** | OpenRouter model page ([link](https://openrouter.ai/typesafe/jev-1.13)), confirmed by [OpenRouter's Jev explainer](https://openrouter.ai/blog/insights/what-is-jev/) |
| **Gemini 2.5 Flash-Lite** (`google/gemini-2.5-flash-lite`) by Google | Writes Juan's reply | **$0.10** (₱5.80) | **$0.40** (₱23.20) | OpenRouter models API, checked live ⭐ ([page](https://openrouter.ai/google/gemini-2.5-flash-lite)) |

📌 Gemini 2.5 Flash-Lite also: context window **1,048,576 tokens** (about 1M), images priced the same as text input ($0.10/1M).

**What's a token?** A small piece of text. **1 token ≈ ¾ of a word**, so 100 words ≈ 130 tokens.

### Why two models? (proof it's cheaper)
Jev reads only the latest message, for **$0.042 per million**, and its answer is **free**. That's **2.4× cheaper on input** than Gemini, and **infinitely cheaper on output.** Gemini then gets only the one tool it needs, so its instructions stay short.

### How much does the decision tree save? (live since Oct 22, 2026)

Every message walks **Juan's decision tree** (`src/ai/juan-tree.ts`, picture: `../JuanStack Info/Files/AI_Cost_Juan_Tree.png`). One Jev call answers two levels at once: the **topic** and the **action**.

| Layer | What happens | Gemini? | Cost per message |
|---|---|---|---|
| 0. Fixed replies | "hi", "salamat", "help" → pre-written answer | ❌ | **₱0** |
| 0. Rules | "How much tax do I owe?", "anything to approve?" → sorted by patterns, Jev skipped | ❌ | **₱0** |
| 1. Jev: topic | BIR/money · schedule · **off-topic → blocked** with a fixed reply | ❌ | ₱0.0018 |
| 2. Jev: action (same call) | Tax due, returns to approve, earnings, schedule for today/tomorrow/this week/next week → **answered by our own code**, with the usual card | ❌ | **₱0.0018** (real billed: $0.0000302) |
| 3. Details there? | Expense with no amount, meeting with no day → **next layer blocked** until Juan gets them ("How much, and who did you pay?") | ❌ | ₱0.0018 |
| 4. Open questions, 2307s, full details | Gemini, with **only that topic's tools**; our code computes, Gemini gets the final numbers | ✅ | **₱0.0107** (Jev + Gemini) |
| 5. Confirm | Nothing is saved or sent until the owner taps Confirm | | |

### Cost per message and per month (only Jev and Gemini)
Assume the owner types **30 words ≈ 40 tokens** (1 token ≈ ¾ of a word).

| Part | Tokens | Math | Cost |
|---|---|---|---|
| Jev reads the message + its sorting questions | **718** (real billed call) | 718 × $0.042 ÷ 1M = $0.0000302 | **₱0.0018** |
| Gemini reads message + Juan's instructions + calendar | ≈ 1,220 | 1,220 × $0.10 ÷ 1M = $0.000122 | ₱0.0071 |
| Gemini writes a 60-word reply | ≈ 80 | 80 × $0.40 ÷ 1M = $0.000032 | ₱0.0019 |
| **One full chat** | | $0.000184 × ₱58 | **₱0.0107** |

| Path | Cost | vs full chat |
|---|---|---|
| Open question (Jev + Gemini) | ₱0.0107 | n/a |
| Our code answers (Jev only) | ₱0.0018 | **84% less** |
| Fixed reply / clear request (no AI) | ₱0 | **100% less** |

| Monthly per user (every chat at ₱0.0107) | Chats | Cost | Share of plan |
|---|---|---|---|
| Free user | capped at ₱0.25 a week | ≈ ₱1 max | n/a |
| Solo, normal (10/day × 22 workdays) | 220 | **₱2.35** | 0.8% of ₱299 |
| Team, normal (3 users) | 660 | ₱7.05 | 0.7% of ₱999 |
| Solo, heavy (50/day × 30) | 1,500 | ₱16.02 | 5.4% of ₱299 |
| Most possible (100/day cap × 30) | 3,000 | ₱32.04 | 10.7% of ₱299 |

Gemini's ≈1,300 tokens per call matches our OpenRouter dashboard (51K tokens over 39 calls). ₱58 = $1 and ₱0.25/week are the live settings in our database. Picture: `../JuanStack Info/Files/AI_Cost_Juan_Tree.png`.

**Say this:**
> "Juan uses two models. Jev, a tiny model, sorts each message for less than 2 centavos per 10 messages. If our own code can answer, like tax due or tomorrow's schedule, that's it: 84% cheaper. Only open questions go to Gemini. A full chat costs about one centavo, so a normal Solo user costs us about ₱2.35 a month in AI, under 1% of their ₱299."

**⚠️ Still to measure:** what **share** of real users' messages end at the tree. The live logs record each message's path (`Juan tree:`), so after pilots you can say "X% of messages never reach Gemini." Until then, don't claim a share; claim the per-message cost.

**Proof it's live:** an automated test against juanstack.com (`tests/e2e/juan-tree.spec.ts`) sent four messages: tax due, schedule tomorrow, expense with no amount, off-topic. All four were answered by the tree, with no Gemini call.

---

## B. Cost of one chat (the "₱0.01 per chat" proof)

> **Updated Oct 22, 2026:** Jev now answers two questions per call (topic + action), and a real 30-word call reads **718 tokens** ($0.0000302). A full chat is now **₱0.0107** instead of ₱0.0103 below. Both round to the concept note's "₱0.01 per chat". Tables below that include receipt scans (₱2.65 / ₱18.09 per Solo user) are the concept note's numbers; chat-only numbers are ₱2.35 normal and ₱16.02 heavy.

Assumption: user types ~30 words, Juan replies ~60 words.

| Part | Tokens | Math | Cost |
|---|---|---|---|
| Gemini input (message + Juan's instructions + 14-day calendar + tool) | ~1,220 | 1,220 × $0.10 ÷ 1,000,000 | $0.000122 |
| Gemini output (60-word reply) | ~80 | 80 × $0.40 ÷ 1,000,000 | $0.000032 |
| Jev input (message + intent categories) | ~570 | 570 × $0.042 ÷ 1,000,000 | $0.000024 |
| Jev output | small | free | $0 |
| **Total** | | **$0.000178 × ₱58** | **≈ ₱0.0103** |

**≈ 1 centavo per chat. ₱1 buys about 97 chats.**

### ⭐ Real proof from our OpenRouter dashboard (testing week)
| Model | Requests | Tokens | Billed | Per request |
|---|---|---|---|---|
| Gemini 2.5 Flash-Lite | 39 | 51K (~1,300 each) | $0.01 | ≈ ₱0.009 |
| Jev 1.13 | 21 | 12K (~570 each) | $0.00 | ≈ ₱0.0014 |

The real ~1,300 tokens per chat matches our estimate of 1,220 + 80.
👉 **Screenshot this tonight:** OpenRouter → Activity → group by Model.

📌 Simple greetings ("hi", "hello") are answered by rules: **₱0, no AI call.**

### One receipt scan (estimate; scanning is in development)
Image ~1,300 tokens + instructions ~500 = 1,800 in → ₱0.0104; ~300 out → ₱0.0070. **≈ ₱0.0174 per scan** (2 centavos). *Source:* Google counts ~258 tokens per 768×768 image tile; a phone photo ≈ 5 tiles.

---

## C. AI cost per user per month

| User | Assumed use | Math | Per month |
|---|---|---|---|
| Solo, realistic | 10 chats + 1 scan a day, 22 workdays | (10 × ₱0.0103 + ₱0.0174) × 22 | **₱2.65** |
| Solo, heavy | 50 chats + 5 scans a day, 30 days | (50 × ₱0.0103 + 5 × ₱0.0174) × 30 | **₱18.09** |
| Free user | capped by the system | ₱1/month (₱0.25/week) | **max ₱1.00** |

**Against a ₱299 plan, AI is under 1% (realistic) to 6% (heavy).**

At 100 paying + 2,000 free users: **₱524/month realistic** (1.4% of ₱36,900 revenue), **₱4,171 worst case** (11%).

📌 Proof the caps are real: the app records each message's actual cost in the database and pauses Juan when a user's weekly allowance is used (`record_ai_spend`, `ai_budget_status` in the database; `src/services/ai-budget.ts`). The app records Jev **conservatively** (assumes a higher price than the real $0.042), so the real bill is lower than what we track.

---

## D. Infrastructure prices (all checked live Oct 22)

| Service | What for | Free tier | Paid | In our budget | Source |
|---|---|---|---|---|---|
| **Supabase** | Database + login | 500 MB database, 50,000 monthly users, **pauses after 1 week inactive** | **Pro $25/mo** (₱1,450): 8 GB disk, 100,000 monthly users, daily backups kept 7 days | ₱1,450 from registration | [supabase.com/pricing](https://supabase.com/pricing) ⭐ |
| **Upstash Redis** | Rate limiting | **500,000 commands/month**, 250 MB | **$0.20 per 100,000 commands** | ₱100/mo (real worst case ~₱71) | [upstash.com/pricing/redis](https://upstash.com/pricing/redis) |
| **Vercel** | Hosting (what we use today) | Free Hobby plan, **non-commercial use only** | **Pro $20/mo per member** (₱1,160) | ⚠️ see note | [vercel.com/docs/plans/hobby](https://vercel.com/docs/plans/hobby) |
| **Netlify** | Hosting (in concept note) | Free plan, 300 credits | Pro $20/mo | Free (concept note) | [netlify.com/pricing](https://www.netlify.com/pricing/) |
| **PayMongo** | Collecting payments | n/a | **GCash 2.23% · Maya 1.79% · Cards 3.125% + ₱13.39** (before VAT) | ~2.5% of revenue | [paymongo.com/en/pricing](https://www.paymongo.com/en/pricing) ⭐ |
| **Domain** juanstack.com | Website address | n/a | ~₱700/year | ₱60/mo | registrar |

### Upstash math (proof ₱100 is enough)
Worst case 370,000 chats × ~3 Redis commands = 1.11M commands. Minus 500K free = 610K × $0.20/100K = **$1.22 ≈ ₱71/month.** Realistic: ~139K commands = **₱0.**

### ⚠️ Hosting: know this before a judge asks
- juanstack.com runs on **Vercel** today (confirmed from the live site). **[VERIFY in Vercel → Settings → Billing whether it's the free Hobby plan.]** If it is: Vercel's own docs say Hobby is **"non-commercial, personal use only."** That's fine for a free beta/prototype.
- The concept note budgets **Netlify's free tier** for the paid launch. Netlify's pricing page doesn't clearly state commercial use either way **[VERIFY before saying "free tier allows commercial use"]**.
- **If asked:** "Today we're on Vercel's free tier for our beta. When we start charging, we'll be on a plan that allows commercial use: either Netlify's free tier or Vercel Pro at $20 a month, about ₱1,160, which our contingency covers."

---

## E. Business numbers (math you can redo on the spot)

| Claim | Math |
|---|---|
| Revenue at 100 users = ₱36,900/mo | 90 × ₱299 + 10 × ₱999 = ₱26,910 + ₱9,990 |
| Fixed costs ≈ ₱4,030/mo (from registration) | Supabase ₱1,450 + CPA ₱2,000 + permits ₱420 + Redis ₱100 + domain ₱60 |
| Break-even at 15 Solo | 15 × ₱299 = ₱4,485 > ₱4,030 + fees + 3% tax + AI |
| We keep ₱273–₱289 per Solo user | ₱299 − ~2.5% payment fee (₱7.50) − AI (₱2.65 to ₱18.09) |
| Customer cost ≈ ₱1,150 | ₱115,000 acquisition budget ÷ 100 paying users |
| Earned back in ~4 months | ₱1,150 ÷ ~₱289/month |
| ₱299 < one penalty | Compromise penalty starts at ₱1,000 (RMO 7-2015) |
| Grant = ₱300,000 | ₱110,000 product + ₱50,000 CPA/legal + ₱115,000 first 100 users + ₱10,000 registration + ₱15,000 contingency |

---

## F. Market & law sources

| Claim | Source |
|---|---|
| 1.24 million MSMEs, 90.4% micro | DTI, *2023 Philippine MSME Statistics in Brief* (2024) |
| 101,368 lawyers | Supreme Court of the Philippines, Lawyers' List (June 2026) |
| 19,168 PCAB-licensed contractors | CIAP Physical Report of Operation, FY 2025 |
| 5,294 real estate establishments | PSA, 2022 ASPBI, Real estate activities (2025) |
| Digital payments 57.4% of retail volume (52.8% in 2023) | BSP, 2024 Status of Digital Payments (2025) |
| SME bookkeeping & tax compliance struggles | Belen et al. (2024), *Asia Pacific Journal of Management and Sustainable Development* |
| 2–5% freemium conversion | Unlock SaaS benchmarks (2026) |
| 10% surcharge, 6% interest for micro/small taxpayers | BIR RR 6-2024 (implementing EOPT Act Sec. 45) |
| Compromise penalty from ₱1,000 | BIR RMO 7-2015 |
| Invoice is the main sales document | RA 11976 (EOPT Act); BIR RR 7-2024 |
| E-invoicing by Dec 31, 2026 (covered taxpayers) | BIR RR 26-2025 |
| 3% percentage tax | RA 11534 (CREATE Act) |
| Data Privacy Act | RA 10173 (2012) |
| How tax software files to BIR (XML, TSP Tool, per-form certification) | BIR RMO 8-2019 ([PDF](https://bir-cdn.bir.gov.ph/local/pdf/RMO%20NO.%208-2019.pdf)) |
| Taxumo, JuanTax are certified TSPs | RMC 20-2026 Annex A (via [Tax Maven PH](https://philippinetaxmaven.com/bir-accredited-tax-software-philippines/)) |

---

## G. Security & technical proof (from our own code)

| Claim | Proof |
|---|---|
| Every business isolated | RLS on **36 tables**, **180 policies** in `supabase/migrations/` |
| Rate limits (10/min per user, 30/min per IP for chat) | `src/lib/rate-limit-rules.ts` |
| Login and sign-up fail closed if Redis is down | same file, `failClosed: true` |
| Daily cap 100 messages, max 2,000 characters | `src/lib/ai-limits.ts` |
| Jev times out after 4 seconds → safe fallback | `src/services/chat-router.service.ts` |
| 2307 link token stored as SHA-256 hash | `src/services/form-2307-portal.ts` |
| Google Calendar token encrypted AES-256-GCM | `src/lib/token-crypto.ts` |
| SAWT file checked against BIR's validation rules | `src/bir/core/sawt.ts`, `src/bir/validators/` |
| 58 automated test files | `src/**/__tests__/` |
| Hosting region | Live site served from Vercel's Singapore edge (`sin1`) |
| Headers: no framing, no referrer | `next.config` (`X-Frame-Options: DENY`, `Referrer-Policy: no-referrer`) |

---

## H. Screenshot these tonight (backup slides)
1. ⭐ OpenRouter Activity: requests and cost by model (proves ~₱0.01 per chat).
2. ⭐ OpenRouter model pages: Jev $0.042 / $0 and Gemini $0.10 / $0.40.
3. ⭐ Supabase pricing (Pro $25) and PayMongo fees.
4. The app: an invoice with its 2307 link, Juan answering "What's due this month?", the sparkle explanation.
5. Supabase dashboard: the table list showing RLS enabled.
