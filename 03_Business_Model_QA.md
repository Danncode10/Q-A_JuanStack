# Business Model Q&A (30% of the score)

> **Owner:** Member B. All numbers are from the Regionals concept note.
> Judges score this on: value proposition, target market, revenue, **scalability**, **sustainability**, feasibility.

---

## Understand it first (so you can explain it your way)

**The whole model in four lines:**
1. **Free plan brings people in.** It's useful (books, invoices, 2307 signing, deadline reminders), so they start trusting us.
2. **Solo (₱299) is where the money is.** It unlocks what saves real time: BIR forms filled in, the industry system, more AI.
3. **Each user costs us almost nothing** (~₱10/month), because the AI is cheap and the tax math is our own code.
4. **So we only need 15 paying users to cover costs.** Everything after that is profit.

**Why the costs are so low (judges love this):** most apps that use AI pay for it on *everything*. We only use AI for conversation. The heavy work, tax math, forms, invoices, runs on our own code, which costs nothing extra per user. One chat ≈ one centavo.

**The math, so you can redo it if asked:**
- 100 paying users = 90 Solo × ₱299 + 10 Team × ₱999 = ₱26,910 + ₱9,990 = **₱36,900/month**
- Break-even: fixed costs ~₱4,030/month (database ₱1,450 + CPA ₱2,000 + permits ₱420 + domain ₱60 + rate limiting ₱100). 15 × ₱299 = **₱4,485**, which covers it with fees and tax.
- **Payback:** ₱115,000 of the grant is for getting customers → **₱1,150 per paying user**. Each user leaves us ~₱289/month → **we earn it back in about 4 months.**
- **Yearly value of one Solo user:** ~₱289 × 12 ≈ **₱3,400** margin.

---

## Questions

### 1. "How do you make money?"
💡 Subscriptions; free plan to attract, Solo to earn.
🗣 "Monthly subscriptions. People start free, and when they want BIR forms filled in and their industry system, they move to Solo at ₱299 a month. Teams pay ₱999. Each Solo user costs us about ₱10, so almost all of it is margin."
📌 Free ₱0 · Solo ₱299 · Team ₱999 + ₱89/extra user (3 included). Payments via PayMongo (GCash, Maya, card, ~2.5% fee).
↪️ *"Future revenue?"* "Annual plans, premium add-ons, and a reseller program for accounting firms who manage many clients."

### 2. "Why ₱299?"
💡 Priced against the pain, not against the cost: cheaper than one penalty.
🗣 "It's under ₱10 a day, and less than the ₱1,000 minimum penalty for a single late BIR return. If Juan saves you one penalty a year, it has already paid for itself."
📌 Compromise penalty starts at ₱1,000 + 10% surcharge + 6% interest (micro/small taxpayers). A part-time bookkeeper costs thousands a month.
↪️ *"Isn't it too cheap? Why not ₱999?"* "We want to win small professionals first, and ₱299 is an easy yes. The Team plan at ₱999 captures bigger offices. We can raise prices once the CPA-certified forms prove the value."
↪️ *"Too expensive for micro businesses?"* "That's why there's a real free plan, not a trial."

### 3. "When do you break even?"
💡 Very early: 15 users.
🗣 "At 15 paying Solo users once we're a registered business. During the pilot, our only costs are the domain and rate limiting, so even one paying user covers it. We register at 30 users, so we're already profitable when the bigger costs start."
📌 Fixed costs ~₱4,030/month. 15 × ₱299 = ₱4,485. If every user maxes their AI, 17 users.
⛔ Don't say "we're profitable from day one." Say "break-even at 15."

### 4. "Why would anyone upgrade when there's a free plan?"
💡 Free builds the habit; the forms are what they pay for.
🗣 "Free keeps their books and collects 2307s. But the moment the quarter ends, they want the BIR forms filled in, and that's Solo. The free plan earns trust; the quarter-end earns the upgrade."
📌 Free has limited monthly chats; Solo adds BIR forms, full industry system, more chats and scans.

### 5. "What if your conversion rate is lower than 2–5%?"
💡 Survival doesn't depend on conversion; only speed of growth does.
🗣 "Our break-even depends on 15 paying users, not on free users. A lower conversion only slows growth. And our first users come from direct pilots and referrals, which convert much better than ads."
📌 Free users are capped at ₱1 of AI a month each, so many free users can't sink us.

### 6. "Won't free users cost you too much?"
💡 It's capped by the system.
🗣 "Each free user gets ₱1 of AI a month, and the app enforces it. Even 2,000 free users cost at most ₱2,000, realistically about ₱200, because most go inactive."
📌 The weekly AI budget is enforced in the database; when it's used, Juan pauses but the rest of the app keeps working.

### 6b. "How do you keep AI so cheap?"
💡 A decision tree: most requests never reach the big model.
🗣 "A decision tree. A tiny model called Jev sorts each message for a fraction of a centavo. Common requests, like 'how much tax do I owe' or 'what's on my schedule tomorrow,' are answered by our own code with no big model. Off-topic questions are blocked. Only open questions reach Gemini. A full chat costs about one centavo; a normal Solo user costs us about ₱2.35 a month in AI."
📌 Per message: full chat ₱0.0107 · our code answers (Jev only) ₱0.0018, 84% less · fixed replies ₱0. Proof: file 10, section A, and `Files/AI_Cost_Juan_Tree.png`.
⛔ Don't claim "X% of messages skip Gemini" yet. That share isn't measured.

### 7. "What if AI prices go up?"
💡 AI is a small part of the cost, and we can switch models.
🗣 "AI is only the conversation; the tax math runs on our own code. And we reach the AI through one gateway, so we can switch to a cheaper model without changing the app."

### 8. "How will you use the ₱300,000?"
💡 Four buckets, and the biggest is getting customers.
🗣 "Four parts. ₱110,000 to make the product production-ready: online services, security testing, mobile app publishing. ₱50,000 for a licensed CPA to certify our tax rules. ₱115,000 to get our first 100 paying businesses: outreach, pilots, ads. And ₱25,000 for business registration and contingency."
↪️ *"₱3,000 per user, isn't that expensive?"* "Only ₱115,000 is for getting customers, which is about ₱1,150 per paying user. Each one gives us about ₱289 a month, so we earn it back in about 4 months. The rest builds things we only pay for once: certification, security, mobile apps."

### 9. "Why spend ₱40,000 on a CPA?"
💡 Trust is the product.
🗣 "Because people are trusting us with their taxes. A licensed CPA approves every computation before we charge anyone, and reviews new BIR rules every quarter. Without that, we'd just be another app guessing."
📌 ₱24,000 one-time review + ₱16,000 quarterly checks (4 × ₱4,000).

### 10. "Is it sustainable after the grant?"
💡 Yes; the grant speeds us up, it doesn't keep us alive.
🗣 "Yes. At 100 paying businesses we net about ₱24,000 to ₱27,000 a month. The grant makes us faster and safer, not dependent."
↪️ *"Your profit excludes salaries."* "Correct, and we state it in the note. We're students building it ourselves this year. Salaries come from revenue after 100 users."

### 11. "Why would they keep paying? (churn)"
💡 Taxes come back every quarter, and their history is in JuanStack.
🗣 "Taxes are every quarter, forever. Once their invoices, 2307s and transactions are in JuanStack, next quarter's forms are already half-done. Leaving means going back to spreadsheets."

### 12. "How do you get customers, and how much does it cost?"
💡 Cheap channels first; paid ads only grow with revenue.
🗣 "Our first users come through NVSU Sabatan and the businesses we interviewed, which costs almost nothing. Then professional associations, DTI Negosyo Centers and seminars. Referrals earn a free month, and Facebook ads grow only as revenue grows."

### 13. "Can this scale as a business?"
💡 Same core, many industries; costs barely grow per user.
🗣 "Yes, two ways. Every new user costs us about ₱10, so margins stay high as we grow. And every new industry, clinics, vets, sari-sari stores, reuses the same core, so we're not building a new product each time."

---

## ⛔ Traps
- **Use 1.24 million MSMEs** (concept note, DTI 2024). The pitch script says 1.21 million; don't mix them.
- Don't invent customer numbers or revenue you don't have.
- Don't say "no competition" or "guaranteed."
