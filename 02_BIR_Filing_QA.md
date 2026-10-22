# JuanStack: "Can you file to BIR?" Q&A

> **Bottom line:** We can't file to BIR today, and we shouldn't claim we can. Filing is turned off (grayed out) in our beta on purpose. Direct filing needs a **BIR certification** that only a registered business can apply for. That's a real, known path, and Taxumo already went through it.
> Researched Oct 22, 2026. Sources at the bottom.

---

## 1. How it actually works (for you)

There are only **three legal ways** to file a BIR return electronically:

| Channel | Who runs it | How it works |
|---|---|---|
| **eBIRForms** | BIR | The taxpayer fills BIR's own form (offline app is Windows-only, or online) and submits it. |
| **eFPS** | BIR | BIR's online filing and payment system, mainly for bigger/required taxpayers. |
| **Certified Tax Software Provider (TSP)** | Private companies like **Taxumo** and **JuanTax** | Their software fills the form, then submits it to BIR for you. |

### How a TSP like Taxumo sends data to BIR

It's **not JSON and not a public API.** Per BIR's rules (RMO No. 8-2019):

1. The company must be **registered with BIR** and apply on BIR's **eTSPCert System** with a sworn application and a non-disclosure agreement.
2. If accepted, BIR gives them a **"TSP Tool"**: a standalone program that loads the return as an **XML file**, checks it, **encrypts** it, compresses it and sends it to BIR.
3. Each BIR form has an official **file structure** (the exact list of fields). The TSP must follow it exactly.
4. They have **90 days** to pass BIR's testing. BIR checks only that the file format and structure are correct.
5. BIR certifies **each form separately**. Taxumo is certified for about 9 forms (e.g. 1701A, 2551Q, 2550Q, 0619E, 1601EQ). JuanTax has about 16.
6. For **payments**, the TSP must partner with an **Authorized Agent Bank** (AAB); payment data goes as **CSV** files over secure file transfer (SFTP).
7. When BIR releases a new version of a form, the TSP must **re-test within 60 days** or lose certification.
8. The TSP must follow the **Data Privacy Act** and is responsible for any breach.
9. The taxpayer is still responsible for the accuracy of what they entered. The TSP is responsible for the software computing correctly.

**So in one line:** a TSP turns your return into BIR's official XML format and sends it through BIR's own encrypted tool, after BIR tests and certifies it per form.

### What JuanStack does today (true)

- Computes the numbers for **2551Q and 1701Q** in fixed, rules-based code (our BIR engine), and prepares a **draft return** for the user to review.
- Generates a **SAWT file** (the list of 2307 tax credits that goes with the 1701Q) in BIR's **.DAT format**, and checks it with a built-in copy of the rules from BIR's own **Alphalist Validation Module**. A file that fails the check is never offered for download.
- **Approve and File buttons are grayed out** in beta.

⚠️ The SAWT format is not yet confirmed against BIR's actual Windows validation program, and the tax numbers aren't CPA-certified yet. So say "prepares," never "certified" or "BIR-ready."

---

## 2. What to say

**Q1. "Can your app file directly to BIR?"**
> "Not yet, and that's deliberate. Filing is turned off in our beta. To submit directly, software has to be certified by BIR as a Tax Software Provider, like Taxumo and JuanTax. Today JuanStack prepares the return so the owner can file it through eBIRForms. Getting certified is on our roadmap once we're a registered business."

**Q2. "How would you connect to BIR? Do they have an API?"**
> "There's no open API. BIR certifies software providers one form at a time. The provider formats each return as an XML file following BIR's official file structure, and BIR's own tool encrypts and submits it. Payments go through a partner bank. Our tax engine already produces structured data per form, so producing that XML is the next step."

*If they push:* "We already generate one BIR file today, the SAWT .DAT file for 2307 credits, and validate it against BIR's published rules before the user can download it."

**Q3. "So what's the point if you can't file?"**
> "Filing is the last 5 minutes. The hard part is the whole quarter before it: tracking income, invoices, 2307s from clients, and computing the tax. That's what JuanStack automates. Then the owner files the numbers we prepared, in minutes instead of a day."

**Q4. "How long would certification take?"**
> "BIR gives a 90-day testing window per application after it's accepted. Before that, we need to register the business with BIR, which is part of our next steps."
> (Don't promise a launch date.)

**Q5. "What makes you different from Taxumo then?"**
> "Taxumo is a filing tool: you bring the numbers, it files. JuanStack is the back office before filing: invoices, 2307 collection from clients, schedule and an AI assistant, with industry packs for lawyers, contractors and property businesses. Taxumo proves the certification path works; we'd use the same path."

**Q6. "Who's liable if a return is wrong?"**
> "Under BIR's rules, the taxpayer is responsible for the accuracy of what they enter, and the software provider for computing correctly. That's why we're getting our tax engine reviewed by a CPA, and why the owner reviews and approves every return."

---

## 3. Don't say

| Don't say | Say instead |
|---|---|
| "We file to BIR." | "We prepare the return; filing is off in beta." |
| "We're BIR-accredited / certified." | "Certification as a TSP is on our roadmap." |
| "We use BIR's API." | "BIR certifies providers per form; returns go as XML through BIR's tool." |
| "Our forms are BIR-ready." | "Our forms are prepared for the owner to review." |
| "We'll be certified by [date]." | "BIR's testing window is 90 days once accepted." |

---

## 4. Check before saying (if you have time)

- The certified list (Taxumo, JuanTax, etc.) comes from **RMC 20-2026 Annex A**, as quoted by a third-party site. Fine to say "Taxumo and JuanTax are certified." Don't quote exact form counts unless asked.
- RMO 8-2019 is from 2019. The process may have been updated since, so say "BIR certifies providers," not the exact steps, unless a judge asks how.

---

**Sources**
- [BIR RMO No. 8-2019: Certification of Electronic Tax Return Filing and/or Payment Solutions](https://bir-cdn.bir.gov.ph/local/pdf/RMO%20NO.%208-2019.pdf)
- [BIR Job Aid for Tax Software Providers (2013)](https://bir-cdn.bir.gov.ph/BIR/pdf/75037Annex%20I_%20Job%20Aid%20for%20Tax%20Software%20Provider%20v1.0%2006192013.pdf)
- [Tax Maven PH: BIR-approved Tax Software Providers list (RMC 20-2026 Annex A)](https://philippinetaxmaven.com/bir-accredited-tax-software-philippines/)
- [Taxumo: eBIRForms](https://www.taxumo.com/blog/ebirforms/)
- [Taxumo: Filing and paying on eFPS under EOPT](https://taxumo.com/blog/filing-and-paying-of-taxes-under-eopt/)
