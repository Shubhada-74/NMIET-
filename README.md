# EcoPilot AI — Autonomous Campus Resource Intelligence Platform

> **Tagline:** “Smarter Campuses. A Greener Tomorrow.”  
> **Brand Promise:** Monitor. Analyze. Optimize. Sustain.

EcoPilot AI is a production-minded multi-tenant campus sustainability and resource intelligence platform. It bridges the gap between passive smart meters and verified physical savings through an evidence-driven, explainable, closed-loop operational workflow:

$$\text{MONITOR} \longrightarrow \text{DETECT} \longrightarrow \text{INVESTIGATE} \longrightarrow \text{PRIORITIZE} \longrightarrow \text{ACT} \longrightarrow \text{VERIFY} \longrightarrow \text{SUSTAIN}$$

---

## 1. Hackathon Evaluation Parameter Highlights

| Dimension | Implementation Details |
| :--- | :--- |
| **A. Security** | Strict multi-tenant isolation, default-deny data model, negative RBAC tests preventing unprivileged task approvals, bounded query limits ($\le 72$ hours), and no secrets in client builds. |
| **B. Efficiency** | Deterministic statistical anomaly engine (Median Absolute Deviation) resolves anomalies before invoking costly LLMs. Zero unbounded loops. |
| **C. Technology Implemented** | React 18, TypeScript (strict), Tailwind CSS, Recharts, Lucide, Vitest, robust statistical baselines, and bounded Gemini tool schemas. |
| **D. Performance** | Sub-25ms deterministic anomaly processing, $<100$ms UI state updates, zero external latency in offline Demo Mode. |
| **E. Google Technology Used** | Gemini API structured tool-calling architecture, Google AI Studio prompt engineering, Firebase default-deny Firestore isolation design. |
| **F. Originality & Real-World Impact** | **Closed-loop verification**: Compares empirical post-intervention meter telemetry against pre-intervention baselines, refusing to hallucinate savings if data is missing. |

---

## 2. Institutional Demo Profile

- **Primary Demo Profile:** **NMIET (Nutan Maharashtra Institute of Engineering & Technology), Talegaon Dabhade, Pune, India**
  - *Disclaimer:* Used for demonstration modeling. Does not represent an official partnership or direct live BMS feed without prior institutional integration.
  - Facilities modeled: Mechanical Engineering Workshop, Computer Engineering & AI Server Lab, Sahyadri Boys Hostel Block A, Central Administration Wing.
- **Fictional Peer 1:** Apex Institute of Technology (Urban high-density research labs).
- **Fictional Peer 2:** Greenfield Polytechnic University (Rural residential agro-campus).

---

## 3. Quickstart & Local Setup

### Prerequisites
- Node.js LTS (v20.x or higher)
- npm (v10.x or higher)

### Installation
```bash
# Clone or navigate to the workspace
cd ecopilot-ai

# Install dependencies
npm install

# Run automated test suite (13/13 passing)
npm run test

# Launch development server
npm run dev
```
The application will start on `http://localhost:3000` with the primary logo properly proportioned in the top-left header.

---

## 4. Environment Configuration (`.env.example`)

EcoPilot AI runs in **Deterministic Demo Mode out-of-the-box** without requiring paid cloud subscriptions or API keys.

To enable live Google Gemini Cloud API investigation:
```bash
cp .env.example .env
```
Populate `.env`:
```env
VITE_GEMINI_API_KEY=your_gemini_api_key_here
VITE_FIREBASE_PROJECT_ID=your_firebase_project_id
```
*(Alternatively, configure the Gemini API key directly in runtime via the **Trust & Security** panel in the UI).*

---

## 5. The 6-Minute Demonstration Script

1. **Brand & Identity:** Observe the EcoPilot AI emblem at the top-left, the campus selector, and the status indicators.
2. **Overview Dashboard:** Review high-level electricity (kWh) and water (kL) aggregates, commercial tariffs, active anomalies, and verified daily savings.
3. **Live Monitoring:** Open building-level telemetry. Toggle between Electricity and Water. Observe the purple dashed line representing the rolling median baseline and the red dotted upper anomaly envelope.
4. **Waste Detective:** Inspect flagged anomalies. Point out:
   - **Computer Center HVAC Override:** 138 kWh/h consumed overnight during unoccupied hours (+294% deviation).
   - **Sahyadri Hostel Water Leak:** Nocturnal unbroken baseflow of 485 L/h between 02:00 and 05:00 (+2,325% deviation).
5. **Evidence-Based AI Investigator:** Launch an investigation on the Computer Center anomaly. Inspect the agentic tool execution logs (`get_resource_history`, `get_baseline`, `get_occupancy_schedule`, `calculate_resource_deviation`). Review ranked physical hypotheses, evidence citations, and explicit uncertainty disclosures.
6. **Action Planner & Approval Gate:** Click *Create Task in Action Planner*. Show that tasks begin in `Pending Approval`. Switch to `Viewer` role to demonstrate negative security testing (cannot approve). Switch back to `Facility Manager` and approve. Click *Submit Completion Evidence* with technician work order sign-off.
7. **Savings Simulator:** Move the interactive sliders to show transparent what-if calculations for tariff adjustments, avoided metric tons of CO₂e, and equivalent trees planted. Note the mandatory `SIMULATED / ESTIMATED` disclaimer.
8. **Closed-Loop Verification:** Open the Verification report for the completed Admin Corridor Lighting task. Show the empirical comparison bar chart proving an observed -37.8% drop (-28.3 kWh/h). Show the Hostel task in `Awaiting Verification` to demonstrate that the engine refuses to invent unverified savings.
9. **Campus Benchmarking:** Review normalized intensity metrics (kWh/student-day, L/student-day, BEI kWh/m²) with zero raw data leakage between tenants.
10. **Data Sources & Security Panel:** Show CSV upload dropzone, downloadable sample CSVs, and cryptographic audit log. Click *Reset Demo* to restore default seed data.

---

## 6. Architecture & Tech Stack

```
ecopilot-ai/
├── public/
│   └── ecopilot-logo.png        # Official high-resolution logo (1024x1024)
├── src/
│   ├── lib/
│   │   ├── analytics/           # Deterministic statistical engines
│   │   │   ├── baseline.ts      # Rolling Median & Median Absolute Deviation (MAD)
│   │   │   ├── wasteDetective.ts# Unoccupied waste & continuous nocturnal leak rules
│   │   │   ├── savingsCalculator.ts # Energy, water & emissions math
│   │   │   └── verificationEngine.ts # Pre vs post intervention empirical delta
│   │   ├── ai/
│   │   │   └── geminiClient.ts  # Bounded Gemini tool calling + offline fallback
│   │   ├── data/                # Calibrated seed data, demo institutions, sample CSVs
│   │   └── storage.ts           # Tenant-isolated persistence with audit logs
│   ├── components/
│   │   ├── layout/              # Header (top-left logo), Sidebar, Collapsible nav
│   │   └── views/               # The 10 Core Application Workflows
│   ├── test/                    # Comprehensive Vitest Automated Test Suite
│   ├── types/                   # Strict TypeScript domain interfaces
│   ├── App.tsx                  # Master application orchestrator
│   └── main.tsx
├── ARCHITECTURE.md              # Detailed architecture specifications
├── SECURITY.md                  # Threat model, controls, negative test results
├── package.json
└── vite.config.ts
```

---

## 7. Known Limitations & Roadmap

- **Sub-circuit disaggregation:** Currently monitors main distribution feeders. Future versions can integrate Non-Intrusive Load Monitoring (NILM) for individual appliance signatures.
- **Weather Normalization:** Degree-day regression (CDD/HDD) can be layered onto the pre/post verification engine.
- **Hardware Integrations:** Physical BACnet/Modbus IoT gateways for direct campus BMS communication.

