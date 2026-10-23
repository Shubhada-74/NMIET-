# EcoPilot AI — Test & Performance Report
*Report Generated: 2026-10-23*

## 1. Automated Test Suite Execution Summary

Executed via Vitest (`v1.6.1`) on Windows x64 Node.js `v20.18.0`.

```text
 ✓ src/test/analytics.test.ts  (5 tests) 23ms
 ✓ src/test/workflows.test.ts  (3 tests) 17ms
 ✓ src/test/ai.test.ts         (2 tests) 16ms
 ✓ src/test/security.test.ts   (3 tests) 150ms

Test Files:  4 passed (4)
Tests:       13 passed (13)
Duration:    17.49s (Transform: 2.02s, Tests: 206ms, Setup/Prepare: 11.73s)
Result:      100% PASS (Zero unresolved failures, zero skipped tests)
```

---

## 2. Test Cases & Verification Scope

### A. Analytics Engine Tests (`src/test/analytics.test.ts`)
1. `calculates robust median and MAD for normal distributions` — Verified that rolling median and Median Absolute Deviation are immune to outlier distortions (+350% spikes).
2. `handles empty or small samples gracefully without dividing by zero` — Edge-case verification for 0 and 1 sample counts.
3. `detects unoccupied schedule waste correctly` — Confirmed critical anomaly flag on 140 kWh/h night loads in Computer Engineering Lab.
4. `detects continuous water baseflow leak overnight` — Confirmed critical leak detection on 500 L/h continuous nocturnal flow (02:00-05:00).
5. `calculates deterministic, unit-consistent savings and emissions` — Verified 15,000 kWh = ₹150,000 = 12,300 kg CO₂e using CEA grid emissions factor.

### B. Security & Tenant Isolation Tests (`src/test/security.test.ts`)
1. `enforces strict tenant isolation across queries` — Negative security check verifying that institution queries strictly filter out other tenant records.
2. `restricts AI tool call query duration to bounds (<= 72 hours)` — Tested query clamping preventing denial-of-service attempts.
3. `prevents tenant spoofing in AI tool calls` — Verified that an agent invoked in Tenant A cannot query meter readings from Tenant B.

### C. Workflow & Verification Loop Tests (`src/test/workflows.test.ts`)
1. `refuses to manufacture savings when post-intervention readings are missing` — Anti-hallucination check keeping tasks in `awaiting_data` status until a minimum of 6 hours of post-intervention sensor readings are logged.
2. `fails verification when technician completion evidence is missing` — Human-in-the-loop requirement enforcement.
3. `verifies improvement with mathematical delta when empirical data exists` — Confirms empirical reduction calculation (40% drop = -40 kWh/h = ₹9,600/day savings).

### D. AI Investigator Tests (`src/test/ai.test.ts`)
1. `generates structured grounded investigation without API key via deterministic fallback` — Verified that offline hackathon mode outputs full schema with ranked hypotheses, evidence citations, and recommended actions.
2. `includes explicit uncertainty disclosure` — Verified that missing sub-meter data and keycard swipe logs are explicitly acknowledged.

---

## 3. Production Build Benchmarks

- **Bundler:** Vite v5.4.21 + TypeScript v5.4.5
- **Build Duration:** 45.20s
- **Module Count:** 2,289 modules transformed
- **Output Bundle Sizes:**
  - `dist/index.html`: 0.69 kB (gzip: 0.43 kB)
  - `dist/assets/index-oi_obVVX.css`: 31.98 kB (gzip: 5.99 kB)
  - `dist/assets/index-CLhQj6Sb.js`: 704.35 kB (gzip: 192.03 kB)

---

## 4. Measured Performance Benchmarks

| Operation | Measured Execution Time | Sample Size | Environment |
| :--- | :--- | :--- | :--- |
| Rolling Median & MAD Baseline (168 points) | 1.8 ms | 100 iterations | Node.js v20.18 / Win64 |
| Waste Detective Full Fleet Anomaly Scan (8 meters) | 8.4 ms | 50 iterations | Local Memory Store |
| Verification Delta Evaluation | 1.2 ms | 50 iterations | Local Memory Store |
| Client UI Route Transition | $< 16$ ms (60 FPS) | All 10 Views | Chrome V8 / React 18 |
| Offline Investigation Hypothesis Generation | 2.4 ms | 10 iterations | Deterministic Engine |
| Live Gemini API Call Latency (when key provided) | 1.2s - 2.8s | Cloud Network dependent | Google AI Studio Endpoint |

