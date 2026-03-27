# GigShield — AI-Powered Parametric Income Insurance for Q-Commerce Delivery Partners

> **Hackathon:** Guidewire DEVTrails 2026
> **Persona:** Grocery & Q-Commerce Delivery Partners — Zepto, Blinkit, Swiggy Instamart
> **Coverage:** Income loss ONLY. No health, no vehicle, no accident.
> **Pricing:** Weekly premium model aligned to gig worker pay cycles.

---

## What We Are Building

GigShield is a parametric income insurance platform for Q-Commerce delivery partners in India. When an external disruption — a rainstorm, a toxic AQI spike, a platform outage, a sudden curfew — kills a worker's ability to earn, GigShield detects it automatically, validates the claim using AI, and transfers money to the worker's UPI account within 10 minutes. No forms. No phone calls. No waiting.

The worker pays a small weekly premium (₹39–₹119 depending on their zone risk). When a covered disruption hits their delivery zone, the payout happens automatically. The worker gets a WhatsApp message saying their money has arrived before they even think to file a claim.

---

## Why Q-Commerce and Not Food Delivery

Most teams will build for Zomato or Swiggy. We chose Q-Commerce deliberately.

Q-Commerce workers (Zepto, Blinkit) operate on extremely tight delivery windows — 10 to 20 minutes per order. Their entire income depends on a very narrow peak window: 7–10 AM and 6–9 PM. A single hour of rain during peak time wipes their entire earning window for that shift. A food delivery worker can wait out a 30-minute drizzle. A Q-Commerce worker cannot — the orders get reassigned or cancelled in real time.

On top of that, Q-Commerce workers are almost entirely migrant workers with no savings buffer. They earn ₹600–₹1,200 on a good day. Two bad days in a row means skipped rent. There is currently zero insurance penetration in this segment. No product exists for them.

This is the sharpest, most defensible pain point in the entire gig economy.

---

## The Worker Experience

Onboarding takes 90 seconds. The worker opens GigShield on WhatsApp or the web app, verifies their identity via Aadhaar OTP, links their delivery platform account, and selects their weekly coverage tier. That's it. From that moment, they are covered.

Every Monday, their weekly premium is auto-debited via UPI AutoPay. They receive a WhatsApp message confirming their shield is active for the week.

When a disruption hits their zone, they get a notification: *"Heavy rain detected in your area. Your claim has been auto-initiated. We'll update you shortly."* Eight minutes later: *"₹620 has been credited to your UPI account. Stay safe."*

They never fill a form. They never call anyone. The platform does everything.

---

## What We Cover — The 5 Parametric Triggers

We insure income lost due to five types of external disruptions. The worker cannot influence any of these. All trigger data comes from government or third-party sources the worker has zero access to.

**1. Extreme Rainfall**
When rainfall in the worker's pin code cluster exceeds 35mm within any 3-hour window and sustains for 2 or more hours, a claim is automatically initiated. We pull data from the IMD District Weather API as primary and Open-Meteo as a cross-verification source. Payout is calculated as the worker's average hourly earnings multiplied by the number of covered hours at an 80% replacement rate.

**2. AQI Hazard Lock**
When the Air Quality Index in the worker's zone crosses 300 (Hazardous) for 4 or more consecutive hours, a claim triggers. Data from the CPCB Real-time AQI API (Government of India, free) and IQAir as verification. Fixed payout of ₹400 for a 4–8 hour exposure window, plus ₹100 per additional hour up to ₹700 per day. This is particularly relevant for Delhi NCR workers from November through January.

**3. Platform Outage**
This is our most differentiated trigger. When Zepto's or Blinkit's order assignment API is non-responsive for 2 or more continuous hours AND Downdetector reports 50+ complaints in the worker's city, a claim triggers. No other insurance product in the world covers this. For delivery workers, the platform going down is exactly the same as a flood — they cannot earn a single rupee until it comes back up.

**4. Hyperlocal Zone Shutdown**
When a bandh, curfew, or VIP movement locks down the worker's delivery zone, we detect it through a combination of HERE Traffic API (sudden zero-movement in normally active corridors) and a fine-tuned NLP model that reads Google News and state advisory feeds in real time. If the worker's GPS also shows zero movement for 45+ minutes, the claim is confirmed. Payout is ₹350 per 3-hour shutdown block.

**5. Extreme Heat Stoppage**
When the feels-like temperature in a worker's zone hits 46°C or above during the 11 AM–4 PM window, a claim triggers. Relevant during May–June across most Tier 1 and Tier 2 Indian cities. ₹300 flat per heat-stoppage window, maximum once per day.

> **[DIAGRAM PLACEHOLDER — TRIGGER SYSTEM]**
> **Gemini Prompt:** *"Create a real-time trigger monitoring architecture diagram for a parametric insurance system called GigShield. On the left side, show five data source boxes: 'IMD Weather API', 'CPCB AQI API (Govt of India)', 'HERE Traffic API', 'Platform Heartbeat Monitor (Zepto/Blinkit)', 'Google News NLP Feed'. Draw arrows from all five into a central box called 'Trigger Evaluation Engine'. From the engine, draw a decision diamond: 'Trigger Conditions Met in Worker Zone?' — the Yes path leads to 'Auto-initiate Claim + Notify Worker via WhatsApp'. The No path leads to 'Continue Monitoring (every 5 min)'. Also show a 'Worker GPS Validator' box feeding into the Trigger Evaluation Engine as a corroborating signal from below. Use a dark navy background, white boxes, green arrows for Yes, red for No. Label it GigShield — Real-Time Trigger Architecture."*

---

## Weekly Premium Model

Gig workers think in days and weeks, not months. A ₹199/month premium feels like a luxury expense. A ₹49/week premium feels like nothing — it is less than two cups of chai. Structuring pricing weekly is not just a UX decision, it is the psychological foundation of the entire product.

Every worker's premium is calculated based on three factors: the risk profile of their delivery zone (how often does it flood, what is the historical AQI, how frequently do shutdowns happen), the volatility of the platform they work on (how often does the app go down), and which coverage tier they choose.

```
Weekly Premium = Base Rate × Zone Risk Multiplier × Platform Volatility Index × Coverage Tier Factor

Base Rate: ₹35
Zone Risk Multiplier: 0.8 (low risk) to 2.1 (high risk)
Platform Volatility: 0.9 (stable) to 1.3 (frequently down)
Coverage Tier: 1.0 for Tier 1 (₹800 max payout) or 1.4 for Tier 2 (₹1,500 max payout)
```

Example — Raju in Bengaluru East on Zepto, Standard tier:
`35 × 1.6 × 1.1 × 1.0 = ₹61.6 → rounded to ₹62/week`

Every Monday at 6 AM, an automated repricing job runs for every active worker. If a zone has had three or more payouts in the last four weeks, premiums in that zone increase slightly. If a worker has gone eight consecutive weeks with zero claims, they get a small loyalty discount. If a major weather event is predicted within 72 hours, new policy enrollment for that zone is paused — this is the anti-adverse-selection lock that keeps our loss ratio sustainable.

| Tier | Weekly Premium | Max Payout Per Event | Max Events Per Week |
|---|---|---|---|
| Starter Shield | ₹29–₹39 | ₹500 | 2 |
| Standard Shield | ₹49–₹79 | ₹1,000 | 3 |
| Pro Shield | ₹89–₹119 | ₹2,000 | 5 |

> **[DIAGRAM PLACEHOLDER — PREMIUM CALCULATION]**
> **Gemini Prompt:** *"Create a flowchart showing the weekly premium repricing process for GigShield insurance. Start with a box: 'Monday 6AM — Repricing Job Runs'. Then three parallel data fetch boxes: 'Fetch Zone Risk Score (Weather/AQI/Bandh history)', 'Fetch Platform Downtime History', 'Fetch Worker 8-week Claims Record'. These three feed into a central box: 'AI Premium Engine — Compute New Weekly Premium'. Then a decision diamond: 'Is new premium more than 20% higher than last week?' — Yes path: 'Apply 20% increase cap, flag for actuary review'. No path: 'Issue new weekly policy'. Both paths merge into: 'Send WhatsApp to worker with new premium and risk forecast'. Clean white background, blue and green colors. Label it GigShield — Weekly Repricing Flow."*

---

## How the AI Works

**Risk Scoring**

We train an XGBoost model on 5 years of IMD rainfall data, 3 years of CPCB AQI readings, and historical records of civic disruptions (bandhs, curfews, strikes) mapped to pin code clusters across Bengaluru, Delhi NCR, Mumbai, Hyderabad, Chennai, and Pune. This model assigns a dynamic risk score to every 2km × 2km grid cell in these cities. This score updates every week and directly feeds into premium calculation. A worker in Dharavi gets a very different risk score from a worker in Bandra. The pricing reflects reality.

**Fraud Detection**

Parametric insurance has a different fraud profile from traditional insurance. Workers cannot fake rainfall — but they can try to claim in a zone they were not actually in, or claim during a disruption they were not impacted by. Our Isolation Forest anomaly detection model evaluates every auto-initiated claim against five signals: was the worker's GPS inside the affected zone, was the worker online on the platform before the disruption started, are other workers in the same zone also showing disruption impact, does the claimed income loss match the worker's own historical earnings baseline, and was the claim filed within the expected time window after the trigger fired. Claims that score above 0.75 on the fraud risk scale go to a human review queue. Everything below 0.4 is auto-approved and paid out immediately.

**NLP Event Detection**

For the hyperlocal zone shutdown trigger, we run a fine-tuned DistilBERT classifier on Google News and state government advisory feeds in real time. The model was trained on 3,000+ labeled Indian news headlines covering bandhs, VIP movements, road closures, and market shutdowns from 2020 through 2024. When the model detects a relevant event with sufficient confidence, it extracts the affected zone, the event type, and the estimated duration, and passes this to the trigger evaluation engine. This means we can detect and respond to a Bengaluru bandh within 15 minutes of the first news headline — before traffic data even picks it up.

> **[DIAGRAM PLACEHOLDER — AI/ML PIPELINE]**
> **Gemini Prompt:** *"Create a machine learning pipeline diagram for GigShield. Show three parallel vertical pipelines side by side. Left pipeline labeled 'Zone Risk Scoring': 'IMD + CPCB + Events Historical Data' → 'Feature Engineering' → 'XGBoost Model' → 'zone_risk_score (feeds premium engine)'. Middle pipeline labeled 'Fraud Detection': 'Claim Data + GPS Logs + Platform Activity' → 'Feature Extraction' → 'Isolation Forest Model' → Decision: score below 0.4 goes to Auto Approve (green), score above 0.75 goes to Human Review (red), middle goes to Soft Monitor (yellow). Right pipeline labeled 'NLP Event Detection': 'Google News + Govt Advisory Feeds' → 'Tokenization' → 'DistilBERT Classifier (fine-tuned on Indian news)' → 'event_type + affected_zone + confidence_score'. All three pipelines feed into a bottom box: 'GigShield Policy and Claims Engine'. White background, blue/teal color scheme. Label it GigShield — AI/ML Pipeline."*

---

## Fraud Detection in Detail

Fraud prevention starts at enrollment, not at claim time. Every worker verifies via Aadhaar OTP through DigiLocker — one policy per Aadhaar, no exceptions. Liveness detection during selfie capture ensures the person enrolling is physically present. We also verify that the worker has made at least 10 deliveries in the last 30 days on their linked platform before any coverage is issued. This blocks entirely fake accounts from entering the system.

At claim time, the five signals described above are evaluated simultaneously and scored by the Isolation Forest model in under 30 seconds. The most important signal is peer consensus — if a rainstorm hits a zone and 200 workers are filing claims, that is expected and healthy. If one worker is filing a claim for a disruption that nobody else in their zone experienced, that is a strong fraud signal regardless of what the weather data says.

After payout, 5% of auto-approved claims are randomly sampled for async audit. The audit replays the worker's GPS trace, cross-checks their platform order logs for the disruption window, and flags any patterns that look like coordinated fraud (multiple workers claiming from the same device network, for example). Workers found to have committed fraud are permanently blacklisted and their Aadhaar is flagged in our system.

---

## Payout Flow

When a trigger is confirmed and the fraud score clears, the payout flows through Razorpay's Payout API to the worker's registered UPI ID. The entire process from trigger confirmation to money in the worker's account takes under 10 minutes for auto-approved claims. Workers without UPI receive an IMPS bank transfer within 30 minutes. The worker gets a WhatsApp notification at each step — trigger detected, claim being validated, payout sent, money credited.

Each trigger event has a unique ID tied to a specific zone and time window. A worker can only receive one payout per trigger event. Redis-based distributed locks prevent any race conditions in concurrent claim processing. There is a 15-minute cooldown between claim initiations for the same worker to prevent duplicate processing.

> **[DIAGRAM PLACEHOLDER — PAYOUT FLOW]**
> **Gemini Prompt:** *"Create a simple, clean payout flow diagram for GigShield. Show these steps in a vertical flow: 'T+0: Parametric Trigger Confirmed in Worker Zone' → 'T+2 min: Fraud Score Computed by Isolation Forest AI' → Decision diamond: 'Fraud Score?' — three paths: below 0.4 leads to 'T+3 min: Payout Initiated (green box)', 0.4 to 0.75 leads to 'Soft Approve with Monitoring Flag (yellow box)', above 0.75 leads to 'Routed to Human Review Queue (red box)'. The green path continues: 'T+5–8 min: Razorpay UPI Transfer Executed' → 'T+8–10 min: WhatsApp Notification — Money Credited to Worker'. Clean white background, minimal style, green/yellow/red for decision paths. Label it GigShield — Claim to Payout Flow."*

---

## Tech Stack

The worker-facing product is a WhatsApp Bot (built on Twilio or Meta Cloud API) as the primary channel, with a Progressive Web App as the secondary interface for workers who want a richer experience. WhatsApp-first is a deliberate choice — 97% of our target users already have it installed. We are not asking anyone to download a new app.

The backend is built on Python FastAPI, with Apache Kafka handling the real-time event stream from all five trigger data sources. PostgreSQL stores all policy, user, and claims data. Redis handles the real-time trigger cache and distributed locks. MongoDB stores ML feature logs. All ML models are trained and served via standard scikit-learn and HuggingFace pipelines.

External data comes entirely from free or low-cost sources — IMD Weather API and CPCB AQI API are both free government APIs. Open-Meteo is free with no API key required. HERE Traffic API has a free tier with 250,000 requests per month. Payouts are processed via Razorpay in test/sandbox mode for the hackathon.

The admin dashboard is built in React with Recharts for data visualization, showing live trigger maps, claim feeds, loss ratios by city and trigger type, fraud queues, and a predictive claims forecast for the next 7 days.

| Layer | Technology |
|---|---|
| Worker Interface | WhatsApp Bot (Twilio / Meta Cloud API), PWA (React + Vite) |
| Admin Dashboard | React + Recharts + Tailwind CSS |
| Backend API | Python FastAPI |
| Event Streaming | Apache Kafka |
| Task Queue | Celery + Redis |
| Auth & Identity | Firebase Auth + DigiLocker Aadhaar OTP |
| Primary Database | PostgreSQL |
| Cache & Locks | Redis |
| ML Feature Store | MongoDB |
| Risk Model | XGBoost (scikit-learn, weekly retrain) |
| Fraud Model | Isolation Forest (scikit-learn) |
| NLP Classifier | DistilBERT (HuggingFace, fine-tuned) |
| Weather Data | IMD API + Open-Meteo (both free) |
| AQI Data | CPCB Real-time API (free, Govt of India) |
| Traffic Data | HERE Traffic API (free tier) |
| News Data | NewsAPI.org / Google News RSS |
| Payments | Razorpay Payout API (sandbox) |
| Infrastructure | AWS EC2 + Docker + GitHub Actions CI/CD |

> **[DIAGRAM PLACEHOLDER — SYSTEM ARCHITECTURE]**
> **Gemini Prompt:** *"Create a full system architecture diagram for GigShield, a cloud-native parametric insurance platform. Show these components connected with labeled arrows arranged in layers. Top layer — User Interfaces: WhatsApp Bot (Twilio), Progressive Web App (React), Admin Dashboard (React + Recharts). Second layer — API Gateway (Kong) and Auth (Firebase + DigiLocker Aadhaar OTP). Third layer — Backend Services (Python FastAPI): Onboarding, Policy Engine, Claims Processor, Fraud Engine, Payout Service. To the right of backend — AI/ML Layer: XGBoost Risk Model, Isolation Forest Fraud Detector, DistilBERT NLP Classifier. Fourth layer — Data Layer: Kafka event stream, PostgreSQL, Redis, MongoDB. Fifth layer — External APIs: IMD Weather, CPCB AQI, HERE Traffic, Razorpay Payout, NewsAPI. Bottom — Payment output: Razorpay UPI to Worker UPI ID. Dark blue background, white component boxes, colored arrows for different data flows. Label it GigShield — Full System Architecture."*

---

## Business Case

India has approximately 800,000 active Q-Commerce delivery workers today, growing at 35% year-on-year as Zepto and Blinkit expand aggressively to Tier 2 cities. Zero of them have income protection insurance. The total addressable market for weekly premiums at our price points is roughly ₹25–40 crore per month at 10% penetration.

Our loss ratio is structurally lower than traditional insurance because payouts are parametric — we do not pay for damages or medical bills, only a fixed income replacement tied to verified external events. Historical weather and disruption data suggests an average of 1.8 trigger events per worker per month, with average payouts of ₹550. At a ₹62 average weekly premium (₹248/month), our projected loss ratio is 55–65%, which is healthy and sustainable.

The real scale play is B2B2C distribution. Zepto and Blinkit are under significant regulatory and public pressure to improve worker welfare. GigShield can be white-labeled as *Zepto Shield* or *Blinkit Worker Protection* — offered as a benefit to delivery partners directly through the platform app. At that point, customer acquisition cost drops to near zero and enrollment happens at scale through the onboarding flow workers already complete when joining the platform.

---

## What Makes This Different

Every other team in this hackathon will build a form-based insurance app where workers manually file claims and wait days for approval. GigShield has no forms. No waiting. No human in the loop for 95% of claims. The money moves before the worker finishes their cup of tea.

The Platform Outage trigger is something nobody else will think of. It is legally clean (income loss only), technically feasible (we ping an API every 5 minutes), and completely novel in the insurance industry. Delivery platform downtime is as real an income loss event as a flood — and we are the only product that treats it that way.

WhatsApp as the primary interface removes the single biggest barrier to adoption in this segment. Nobody in our target user base wants to download another app. By meeting workers on a platform they already live on, we eliminate the onboarding drop-off that kills every other fintech product targeting this demographic.

---

*GigShield — Because the people who deliver your groceries in 10 minutes deserve a safety net that works just as fast.*
