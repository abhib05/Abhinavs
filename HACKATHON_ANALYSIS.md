# Hackathon Analysis: Tech for India's Small Merchants

**Sponsors:** n8n (workflow automation and AI agents) · Sarvam AI (Indian-language LLMs, speech and document AI)

---

## 1. What the problem statement actually asks for

| Phrase in the brief | What it means for the judges |
|---|---|
| "Identify a **real and meaningful** problem" | You need to name one specific pain clearly, ideally backed by a stat or a merchant quote. Vague "digital empowerment" pitches lose. |
| "**Measurable** value" | Every pitch needs a number: hours saved, ₹ recovered, % more orders, lower credit cost. Put it in the demo. |
| "More efficient / discover opportunities / improve financial outcomes / compete and grow" | These are the **four scoring lenses**. A strong idea hits at least two. |
| "**Think beyond existing financial and commerce solutions**" | Don't build another UPI app, Khatabook clone or storefront builder. Paytm, PhonePe, Khatabook, Dukaan and ONDC already do that. The new part has to be *how* it works (voice, agents, automation), not *what* it is. |
| "Challenge conventional approaches" | The merchant shouldn't have to learn an app. The tech should fit the merchant's existing habits: WhatsApp, voice notes, paper bahi-khata, phone calls. |
| Sponsors are **n8n + Sarvam AI** | The unwritten rubric: use **Sarvam for vernacular voice/text/docs** and **n8n as the automation/agent backbone**. Using both deeply is close to mandatory. |

### Ground realities of the Indian small merchant (use these in your pitch)
- ~6.3 crore MSMEs, most of them micro (kirana, tailors, repair shops, street vendors, small manufacturers, artisans).
- Most don't work comfortably in English and many have low literacy, but nearly all use **WhatsApp and voice notes**.
- UPI made payments digital, but **the rest of the business** (inventory, credit/udhaar, GST, supplier orders, marketing, schemes) is still on paper or in their head.
- Main pains: **informal credit (udhaar) collection**, **working-capital shortage**, **GST/compliance confusion**, **losing to quick-commerce/e-commerce**, **not knowing about govt schemes**, **stock-outs and dead stock**, **no time for marketing**.
- Formal credit gap: most micro-merchants have **thin credit files**, even though UPI data shows they're creditworthy.

### What each sponsor gives you
**Sarvam AI** (APIs at `api.sarvam.ai`, 10 Indian languages + English, plus code-mixed "Hinglish"):
- **Saarika / Saaras**: speech-to-text, and speech-to-English-text translation. Voice notes become text.
- **Bulbul**: natural Indian-voice text-to-speech. Text becomes a voice reply.
- **Mayura / Sarvam-Translate**: translation, transliteration, code-mixed text.
- **Sarvam-M / Sarvam LLMs**: Indic reasoning and chat, function calling, OpenAI-compatible chat completions.
- **Document intelligence / vision (Sarvam Parse)**: OCR of Indic invoices, handwritten bills, bahi-khata pages.
- Language identification.

**n8n** (self-hosted or cloud):
- Native **WhatsApp Business Cloud**, Telegram, Gmail, Google Sheets, Postgres/Supabase, Razorpay/Stripe, HTTP Request (for Sarvam), Webhook, Cron/Schedule nodes.
- **AI Agent node** (LangChain-based) with tools, memory and vector stores. Sarvam can be plugged in through the OpenAI-compatible model node or HTTP.
- Human-in-the-loop approval steps, error workflows, sub-workflows. These are good for showing agentic workflows that still have guardrails.

> **The winning pattern:** *Merchant speaks in their language → Sarvam understands → n8n agent acts across tools → Sarvam replies in their voice.* No app to install.

---

## 2. Problem space map

| # | Pain area | Scoring lens | Severity | Saturation of existing solutions |
|---|---|---|---|---|
| A | Udhaar / receivables collection | Financial outcomes | 🔴 Very high | Medium (Khatabook, OkCredit do ledgers, but not collection *intelligence*) |
| B | Bookkeeping & stock without typing | Efficiency | 🔴 High | Medium (apps exist, adoption low because of literacy/typing) |
| C | Working capital & credit access | Financial outcomes | 🔴 Very high | Medium (NBFCs exist, but discovery/paperwork is the barrier) |
| D | GST / compliance / paperwork | Efficiency | 🟠 High | Medium (CA-centric tools, English-only) |
| E | Govt scheme & subsidy discovery | Opportunities | 🟠 High | **Low** (almost nobody does this well) |
| F | Competing with quick-commerce | Compete & grow | 🔴 Very high | Low–medium (ONDC exists, onboarding is hard) |
| G | Marketing & customer retention | Grow | 🟠 High | Low for vernacular/voice |
| H | Supplier ordering & procurement | Efficiency, financial | 🟠 Medium-high | Low (Jumbotail, Udaan are marketplaces, not assistants) |
| I | Demand forecasting / local insights | Opportunities | 🟠 Medium | **Low** |
| J | Fraud & scam protection | Financial | 🟠 Medium-high | **Low** |
| K | Collective bargaining / merchant networks | Compete in new ways | 🟢 Medium | **Very low (novel)** |
| L | Skilling / business literacy | Grow | 🟢 Medium | Low |

---

## 3. All candidate solutions

Each one lists **the problem → the solution → how n8n + Sarvam fit → the measurable metric → novelty**.

### 🅰 Financial outcomes

#### A1. "Vasooli Saathi": AI udhaar collection agent ⭐
- **Problem:** Kiranas have ₹20k–₹2L stuck in informal credit. Asking customers to pay is socially awkward, so the money never gets recovered.
- **Solution:** The merchant sends a voice note: *"Ramesh ne 500 ka saamaan liya, agle hafte dega"* ("Ramesh took ₹500 of goods, he'll pay next week"). The system logs it automatically. On schedule it sends the customer polite, escalating reminders **in the customer's language**, as Bulbul voice notes plus a UPI payment link. The merchant doesn't have to ask in person.
- **n8n:** WhatsApp trigger → Sarvam STT → Sarvam-M extracts {customer, amount, due date} → Sheets/Postgres ledger → Schedule node for reminders → Razorpay/UPI link → webhook marks the debt paid.
- **Sarvam:** STT, LLM entity extraction, TTS reminder voice, translation into the customer's language.
- **Metric:** % of udhaar recovered and days-to-recover (DSO).
- **Novelty:** Ledger apps exist. A **voice-first, culturally-polite, automated collection agent** doesn't.

#### A2. "Credit Passport": alternative credit score from voice + UPI data
- **Problem:** Thin-file merchants can't get loans.
- **Solution:** Combine UPI statement parsing, the voice ledger (A1/B1) and stock turnover into a **cash-flow health score** plus a ready-made loan application. Explain the score to the merchant in their own language.
- **n8n:** Statement upload → Sarvam Parse → scoring workflow → generates a PDF profile → matches lenders/schemes (Mudra, PM SVANidhi).
- **Metric:** Loan eligibility and interest rate offered compared with a moneylender (24–60% APR vs 12–18%).
- **Novelty:** High, because it's explainable and vernacular.

#### A3. Cash-flow forecaster & "Should I buy this?" advisor
- The merchant asks by voice: *"Diwali ke liye 50,000 ka stock lu kya?"* ("Should I buy ₹50,000 of stock for Diwali?"). The agent looks at past sales, udhaar due and upcoming festivals, then answers by voice with a risk-adjusted recommendation.
- **Metric:** Fewer cash crunches, less dead stock.

#### A4. Payment fraud & scam shield
- **Problem:** Fake UPI screenshots, "refund" scams, fake soundbox spoofing.
- **Solution:** The merchant forwards a payment screenshot. Sarvam Vision OCRs it, and n8n checks it against the actual bank/UPI webhook. The merchant gets a voice alert: *"Yeh payment aapke account mein nahi aaya"* ("This payment hasn't reached your account").
- **Metric:** ₹ fraud prevented. **Novelty:** High, and it's a real, visceral pain.

### 🅱 Efficiency

#### B1. "Bolo Bahi": voice-first bookkeeping and inventory ⭐
- The merchant talks all day: *"10 kilo chawal aaya, 2 Maggi bika"* ("10 kg rice came in, 2 Maggi sold"). Sarvam STT and the LLM turn this into structured inventory and sales entries. Every evening the merchant gets a **spoken daily summary** (sales, profit, low stock).
- **n8n:** WhatsApp → STT → LLM structured output → Postgres → nightly Cron → Bulbul summary voice note.
- **Metric:** Minutes of bookkeeping saved per day, stock-out incidents.

#### B2. Bahi-khata digitiser (paper → digital)
- The merchant photographs their handwritten ledger or supplier bills. Sarvam document intelligence extracts the entries (Hindi/Tamil/etc. handwriting) and n8n reconciles them with stock and GST.
- **Metric:** Pages digitised, hours of data entry saved. Good demo moment.

#### B3. "GST Mitra": vernacular compliance agent
- Turns WhatsApp sales and purchase bills into GSTR-1/3B drafts, sends deadline reminders as voice notes, and answers questions like *"Composition scheme mein jaana chahiye?"* ("Should I move to the composition scheme?") in plain language, using RAG on GST circulars.
- **Metric:** Late fees avoided (₹50/day per return), CA fees saved.

#### B4. Smart supplier re-ordering agent
- Low stock is detected (from B1). The agent drafts the order, compares 2–3 distributors' price lists, sends a WhatsApp order to the supplier in *the supplier's* language, and tracks delivery.
- **n8n:** Multi-agent flow with human approval ("Haan bhejo" = "Yes, send it" by voice confirms).
- **Metric:** Procurement cost %, time spent ordering.

### 🅲 Discover new opportunities

#### C1. "Yojana Dost": govt scheme & subsidy matchmaker ⭐ (low competition)
- **Problem:** Billions in MSME schemes go unclaimed (Mudra, PM SVANidhi, PMEGP, Udyam benefits, state subsidies, CGTMSE). Merchants don't know about them, and the forms are in English.
- **Solution:** A 2-minute voice interview builds the merchant's profile. RAG over scheme documents finds eligible schemes, explains them by voice, **auto-fills the forms**, and keeps tracking the application status.
- **n8n:** Agent with vector store (scheme PDFs) → eligibility rules → form-filling sub-workflow → reminders.
- **Metric:** ₹ benefits unlocked per merchant.

#### C2. Hyperlocal demand radar
- Combines festival calendar, weather, local events and aggregated anonymised sales from nearby merchants into proactive voice tips: *"Is hafte baarish hai, chhate aur Maggi ka stock badhao"* ("It's going to rain this week, stock up on umbrellas and Maggi").
- **Metric:** Sales uplift on recommended SKUs.

#### C3. B2B / institutional order finder
- Scans GeM tenders, local institutional needs (schools, hospitals, offices) and bulk-buy requests, matches them to the merchant, and pitches on the merchant's behalf.
- **Novelty:** Very high for micro-merchants.

#### C4. Export / ONDC onboarding assistant for artisans
- Voice-describe a product → Sarvam generates listings in English and regional languages → n8n publishes to ONDC, Instagram and WhatsApp Catalog.

### 🅳 Compete & grow in new ways

#### D1. "Dukaan on WhatsApp": vernacular voice commerce for kiranas vs quick-commerce ⭐
- **Problem:** Blinkit and Zepto are pulling customers away from kiranas.
- **Solution:** The kirana's customers send voice orders in any language (*"Bhaiya 1 litre doodh aur bread bhej do"*, "Brother, send 1 litre of milk and bread"). The AI parses the order, checks stock, confirms the price with a voice reply, sends a UPI link, and alerts the delivery boy. The kirana gets **10-minute delivery like Blinkit, but with its own trust and udhaar**.
- **n8n:** WhatsApp → STT → LLM order parser → inventory check → payment link → delivery dispatch → feedback.
- **Metric:** Orders per day, repeat-customer rate, average order value.

#### D2. AI marketing studio (voice in → campaign out)
- *"Diwali offer banao, kaju 10% off"* ("Make a Diwali offer, cashews 10% off") produces a poster, WhatsApp broadcast text in 3 languages, a Bulbul voice ad and Instagram captions. Target customers are segmented from purchase history ("customers who haven't come in 30 days").
- **Metric:** Campaign reach and win-back rate.

#### D3. "Merchant Collective": group buying & shared logistics ⭐ (most novel)
- **Problem:** Individually, small merchants have no bargaining power.
- **Solution:** An agent aggregates re-order needs across 20–50 nearby kiranas (from B4), negotiates bulk prices with distributors, and coordinates shared delivery. Basically a digital co-operative.
- **Metric:** % procurement cost saved. **Novelty:** Very high. It really is "competing in new ways".

#### D4. Customer review & reputation manager
- Collects voice feedback from customers, auto-responds to Google Maps reviews in the right language, and surfaces complaints to the merchant as a daily voice digest.

#### D5. Price intelligence agent
- Scrapes Blinkit/JioMart/DMart prices for the merchant's top SKUs and suggests competitive pricing or bundles by voice.

### 🅴 Cross-cutting / platform plays

#### E1. "Munshi": the all-in-one AI business manager on WhatsApp ⭐⭐ (recommended umbrella)
One WhatsApp number in the merchant's language that acts as a **digital munim/accountant**. Modules: Bolo Bahi (B1), Vasooli (A1), Re-order (B4), Yojana (C1), and a daily voice briefing. This works as a hackathon pitch because there's one story and many visible wow moments, but **build only 2–3 modules deeply**.

#### E2. Voice IVR for feature-phone merchants
- The same agent reached through a phone call (Exotel/Twilio → n8n webhook → Sarvam STT/TTS). It covers merchants without smartphones. Strong inclusion story.

#### E3. Business coach / literacy bot
- Bite-sized voice lessons (pricing, margins, digital payments, fraud safety) personalised to the merchant's own data.

---

## 4. Evaluation matrix

Scores are 1–5 (5 = best). "Sponsor fit" = how deeply it uses both n8n and Sarvam.

| Idea | Real pain | Measurable | Novelty | Sponsor fit | Demo-ability (36–48h) | **Total** |
|---|---|---|---|---|---|---|
| **A1 Vasooli Saathi (udhaar collector)** | 5 | 5 | 4 | 5 | 5 | **24** |
| **E1 Munshi (umbrella: A1+B1+C1)** | 5 | 5 | 4 | 5 | 4 | **23** |
| **D1 Voice commerce for kiranas** | 5 | 5 | 3 | 5 | 5 | **23** |
| **C1 Yojana Dost (schemes)** | 4 | 5 | 5 | 5 | 4 | **23** |
| **D3 Merchant Collective** | 4 | 5 | 5 | 4 | 3 | **21** |
| B1 Bolo Bahi | 5 | 4 | 3 | 5 | 5 | 22 |
| A4 Fraud shield | 4 | 4 | 4 | 4 | 4 | 20 |
| A2 Credit Passport | 5 | 4 | 4 | 3 | 3 | 19 |
| B3 GST Mitra | 4 | 4 | 3 | 4 | 3 | 18 |
| D2 Marketing studio | 3 | 3 | 3 | 4 | 5 | 18 |
| B2 Bahi-khata digitiser | 4 | 3 | 3 | 4 | 4 | 18 |
| C2 Demand radar | 3 | 3 | 4 | 3 | 3 | 16 |

---

## 5. Recommendation

### 🏆 Build **"Munshi": a voice-first AI munim on WhatsApp**, with three deep modules:
1. **Bolo Bahi**: voice sales/stock/udhaar entry (efficiency)
2. **Vasooli Saathi**: automated, polite, multilingual udhaar collection with UPI links (financial outcomes, and the headline metric)
3. **Yojana Dost**: proactive scheme and loan matching from the data collected (opportunities)

**Why this wins:** it hits three of the four scoring lenses, has a crisp ₹ metric ("recovered ₹X of stuck udhaar"), needs no app or literacy, uses Sarvam for STT, LLM, TTS, translation and OCR, and uses n8n for the agent, schedules, integrations and human-in-the-loop steps. That's deep use of both sponsors.

**Strong alternatives:**
- If you want maximum novelty, go with **D3 Merchant Collective** layered on top of Munshi's re-order data.
- If you want the cleanest demo, go with **D1 Voice commerce for kiranas** ("the kirana's answer to Blinkit").

### Reference architecture
```
Merchant / Customer (WhatsApp voice / text / photo, or phone call)
        │
        ▼
 n8n Webhook / WhatsApp Trigger
        │
        ├─ audio → Sarvam Saarika STT (+ language ID)
        ├─ image → Sarvam Document Intelligence (bills, bahi pages)
        ▼
 n8n AI Agent (Sarvam-M via OpenAI-compatible node)
   tools: ledger_add · stock_update · create_payment_link · schedule_reminder
          · scheme_search (vector store) · supplier_order · daily_summary
   memory: Postgres chat memory per merchant
        │
        ├─ Postgres/Supabase (ledger, inventory, customers)
        ├─ Razorpay / UPI payment links → payment webhook
        ├─ Schedule node → reminders, daily voice briefing
        └─ Human approval node for money-moving actions
        ▼
 Sarvam Mayura (translate to recipient's language) → Bulbul TTS
        ▼
 WhatsApp voice note + text reply
        ▼
 Simple web dashboard (optional, for judges): ₹ recovered, DSO, stock, schemes
```

### 48-hour build plan
| Hours | Work |
|---|---|
| 0–4 | Set up n8n (Docker/cloud), WhatsApp Cloud API test number, Sarvam API key, Supabase schema |
| 4–14 | Voice → STT → agent → ledger loop (Bolo Bahi). Verify Hindi plus one more language. |
| 14–24 | Vasooli: reminder schedule, Bulbul voice note, payment link, paid webhook |
| 24–32 | Yojana: embed 10–15 scheme PDFs, eligibility Q&A, voice explanation |
| 32–40 | Daily voice summary, dashboard with live metrics, error workflows |
| 40–48 | Seed realistic demo data, record a backup demo video, build the pitch deck |

### Pitch tips
- Open with a **real merchant voice note** (Hindi/Telugu/Tamil), not a slide.
- Do the demo **live on a phone**: speak → bot replies in voice → customer's phone gets the reminder → pay → ledger updates.
- Show one number: *"Pilot merchant: ₹18,400 udhaar, 62% recovered in a week"* (simulated or from 2–3 real kiranas you talk to before the event).
- Show the n8n canvas for 20 seconds. Sponsors love seeing their tool as the backbone.
- Mention privacy and guardrails: human approval before money moves, data stays with the merchant, DPDP Act awareness.
- Roadmap slide: IVR for feature phones (E2) → Credit Passport (A2) → Merchant Collective (D3).

### Risks & mitigations
| Risk | Mitigation |
|---|---|
| WhatsApp Cloud API approval delay | Use the test number, or Telegram as a fallback channel in n8n |
| STT errors on noisy shop audio | Ask for confirmation ("₹500 Ramesh ke naam, sahi hai?", "₹500 under Ramesh's name, correct?") before writing |
| Aggressive reminders hurt customer relationships | Tone controls (gentle / firm), merchant approves the first message |
| LLM hallucinating scheme eligibility | Rules-based eligibility check, with the LLM only explaining. Cite the source. |
| Live demo failure | Pre-recorded backup video plus seeded data |
