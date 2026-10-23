# Technical Architecture — EcoPilot AI

EcoPilot AI is built on a modular, decoupled full-stack architecture designed for educational institutions.

---

## 1. Complete Architecture Diagram

```mermaid
flowchart TD
    subgraph DataIngestion ["Data Ingestion & Telemetry"]
        CSV["CSV Ingestion Pipeline (Schema Validated)"]
        Seed["Calibrated Seed Telemetry (168h baseline)"]
        IoT["Future: BACnet / MQTT Smart Meters"]
    end

    subgraph AnalyticalEngine ["Deterministic Analytics Layer"]
        Baseline["Robust Rolling Median & MAD Envelope"]
        Detector["Waste Detective Anomaly Engine"]
        Savings["Deterministic Savings & Tariff Simulator"]
        Verifier["Pre vs Post Empirical Verification Engine"]
    end

    subgraph AIInvestigation ["Agentic Reasoning Layer (Gemini)"]
        BoundedTools["Bounded Tool Whitelist<br/>(get_history, get_baseline, get_schedule)"]
        Synthesizer["Evidence-Grounded Hypothesis Generator"]
        Disclosures["Explicit Uncertainty & Citation Engine"]
    end

    subgraph PersistenceSecurity ["Persistence & Governance"]
        TenantMgr["Tenant Isolation & RBAC Governance"]
        AuditLog["Immutable Cryptographic Audit Store"]
        ActionQueue["Human-in-the-Loop Action Planner"]
    end

    subgraph PresentationLayer ["Responsive User Interface (React 18)"]
        Overview["Overview Dashboard"]
        LiveMon["Live Telemetry & Baseline Overlay"]
        DetectiveUI["Waste Detective & Evidence Modal"]
        InvestigatorUI["AI Root Cause Investigator"]
        PlannerUI["Governed Task Lifecycle"]
        SimUI["What-If Savings Simulator"]
        VerifUI["Closed-Loop Verification Audits"]
        BenchUI["Normalized Cross-Campus Benchmarks"]
        SourcesUI["Data Fleet & Provenance"]
        TrustUI["Trust, Security & RBAC Panel"]
    end

    CSV --> AnalyticalEngine
    Seed --> AnalyticalEngine
    AnalyticalEngine --> Detector
    Detector --> BoundedTools
    BoundedTools --> Synthesizer
    Synthesizer --> ActionQueue
    ActionQueue --> Verifier
    Verifier --> PresentationLayer
    TenantMgr --> AnalyticalEngine
    TenantMgr --> AIInvestigation
    AuditLog --> TrustUI
```

---

## 2. Core Operational Loop Specifications

1. **MONITOR:** Ingests 15-minute resolution electricity (kWh) and water (Litres) telemetry. Tags timestamps with building operating schedules (`isOccupiedPeriod: boolean`).
2. **DETECT:** Evaluates consumption against historical rolling median baselines:
   - Unoccupied schedule consumption (e.g. HVAC/lighting running overnight).
   - Nocturnal water baseflow (leaks never returning to zero between 02:00 and 05:00).
   - Sudden surge (readings exceeding median + 2.5 MAD).
3. **INVESTIGATE:** Gemini agent executes bounded retrieval tools (`get_resource_history`, `get_baseline`, `get_occupancy_schedule`, `calculate_resource_deviation`) to formulate ranked physical hypotheses with cited evidence.
4. **PRIORITIZE:** Action Planner queues tasks in `Pending Approval` with calculated daily recoverable losses and priority ratings.
5. **ACT:** Human facility manager approves tasks and field technicians complete physical work orders with signed verification records.
6. **VERIFY:** Automated verification engine pulls post-intervention meter telemetry, compares against pre-intervention baseline, and calculates empirical reduction percentage. Refuses to conclude without at least 6 hours of real sensor data.
7. **SUSTAIN:** Aggregates verified avoided CO₂e emissions, financial budget recovery, and normalized cross-campus benchmarking indices.

