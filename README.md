# WaterOS / Wildfire Treatability Operating System

**Sovereign Process Architecture deployment specification for post-wildfire drinking-water treatability governance.**

A complete architectural specification encoding the published wildfire-treatability methodology from the University of Waterloo research lineage — dynamic filter-cycle log-reduction mathematics, coagulant demand escalation under post-wildfire DOC loading, zeta-potential anchored coagulation governance, hydrologic window durations for source-water state transitions, and uncertainty-aware data interpretation for pathogen and turbidity enumeration — into deterministic, auditable governance software with an immutable SHA-256 hash-chain Flight Recorder audit trail. The architecture treats wildfire-impacted source-water disturbance not as a raw-water condition but as a treatment-governance condition with explicit reserve states, evidence-burden subsections per certification tier, and structural prohibitions against false-confidence claims from sparse sampling.

---

## Publication Context

This document is the architectural specification authored by [**Sovereign Process Architecture Inc.**](https://github.com/chopinregis/Chopinregis) (Corporation Number 1781822-0, federally incorporated April 2026), built on publicly available research from the University of Waterloo wildfire-treatability research program (Emelko et al. 2003, 2010, 2011; Schmidt et al. 2020), the Water Research Foundation (WRF 5168 Treatment Resilience to Wildfire Events), the Canadian Water Network, and the broader peer-reviewed water treatment science literature. All methodology lineage is fully cited throughout the document (see References). All underlying research cited is in the public domain or otherwise publicly available without use restrictions.

The four architectural invariants, V0–V7 Validator Node Pipeline, six Prohibited Content Rules (PC-01 through PC-06), Agency Gate Checklist, dual-window engine for source-water and treatment-state monitoring, effective log-reduction (μ_LR, σ²_LR) governance mathematics, and Flight Recorder design are the original intellectual property of Sovereign Process Architecture Inc.

This specification is published to establish architectural priority and to make the methodology publicly available to drinking water utilities, watershed management agencies, drinking water regulators, and academic groups working on post-wildfire and source-water resilience in the same problem space.

## License

This methodology specification is licensed under **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International** (CC BY-NC-SA 4.0). The document may be read, adapted, and cited freely with attribution to *Sovereign Process Architecture Inc.* Commercial use of the architectural framework — any system that materially encodes the four SPA invariants, the V0–V7 Validator Node Pipeline, the Prohibited Content Rules, the Agency Gate Checklist, the dual-window engine, or the Flight Recorder design — requires separate written permission from SPA Inc.

The calibration parameters labeled *[engineering estimate]*, *[placeholder pending calibration]*, or carrying inline calibration caveats throughout this specification require empirical validation against utility-specific source-water and treatment-plant data before operational deployment. The architectural guarantees of Section 11 hold by design regardless of the values of any numerical parameter.

## Author

**Regis Benoit Brice Nde Tene** — Lead Architect, Sovereign Process Architecture Inc.

Inquiries: regisndetene@gmail.com  ·  [SPA Inc. profile](https://github.com/chopinregis/Chopinregis)

---





**WaterOS Protocol**

**Wildfire Treatability Operating System**


Master Architectural Specification — University of Waterloo Wildfire-Treatability Methodology Lineage

**Version: v1.0 REV3.1**

Author: Regis Benoit Brice Nde Tene

Lead Architect: Sovereign Process Architecture

Date: 7 March 2026


*REV3.1 scope: This revision adds Section 12 — the historical counterfactual validation chapter — to the epistemic spine established in REV3. Section 12 is grounded in the Lost Creek wildfire (2003, Oldman River headwaters, Alberta) and the four-year post-disturbance record published by Emelko and colleagues (2011). It demonstrates, against a real named event documented in the cited Emelko et al. (2011) research, where WaterOS governance would have intervened before the source-water crisis became visible to operators. A companion case based on the Okanagan Mountain Park Fire (Kelowna, BC, 2003) is in preparation for REV4 pending utility-level chronology sourcing. All other sections carry forward unchanged from REV3.*


**Document Control**

This specification is written as a single-source architectural instrument rather than as a literature review. The objective is to convert the published treatability logic associated with wildfire, source-water disturbance, and dynamic filtration into a deterministic operating system design that a scientist, utility engineer, or partner institution can inspect, challenge, and calibrate. Pinpoint page and table citations for all numerical anchors are pending empirical calibration review; the current reference list identifies document, year, and claim but not page number pending that verification.

**Revision History**

| **Version** | **Date** | **Author** | **Summary** | **Status** |
| - | - | - | - | - |
| v1.0 REV1 | 9 February 2026 | Regis Benoit Brice Nde Tene | Initial master architectural specification based on wildfire-treatability literature, dynamic filter-cycle evidence, treatment-resilience piloting, and uncertainty-aware data interpretation. | Superseded |
| v1.0 REV2 | 18 February 2026 | Regis Benoit Brice Nde Tene | Effective log-reduction mathematics, coagulant demand quantification, Recovery Update event (E10), reserve state thresholds, zeta-potential anchor, hydrologic window durations, Appendix E Calibration Specification. | Superseded |
| v1.0 REV3 | 27 February 2026 | Regis Benoit Brice Nde Tene | Epistemic tightening: PC-06 added, μ\_LR/σ²\_LR notation standardised, inline calibration caveats on reserve thresholds, placeholder PJI thresholds, Agency Score replaced by gate checklist, Appendix B.0 variable dictionary added, Section 7.5 expanded to evidence-burden subsections. | Superseded |
| v1.0 REV3.1 | 7 March 2026 | Regis Benoit Brice Nde Tene | Section 12 added: historical counterfactual validation against the Lost Creek wildfire (2003, Oldman River headwaters, Alberta) using Emelko et al. (2011) as the evidentiary basis. Gate-by-gate WaterOS governance walkthrough. Okanagan/Kelowna companion case deferred to REV4 pending utility chronology sourcing. | Draft for review |


**Table of Contents**

Executive Summary

1.  Problem Definition

2.  Architectural Invariants

3.  System Overview

4.  Event Model

5.  Process Modules

6.  Process Physics

7.  Sovereign Governance Layer

8.  Operating Protocol / Runbook

9.  Governance Gates as Executable Logic

10. Implementation Architecture

11. Validation Plan — Epistemic Boundary

12. Historical Counterfactual Validation — Lost Creek Wildfire (2003)

Appendix A: Event Model and Canonical Schemas

Appendix B: Canonical Mathematics (includes B.0 Variable Dictionary)

Appendix C: Validator Node JSON Contracts

Appendix D: Validation Matrix

Appendix E: Calibration Dataset Specification

References


**Executive Summary**

Post-wildfire drinking-water treatment has a structural failure that is often misdescribed as a chemistry problem. The chemistry matters, but the recurring failure is infrastructural: consequential decisions are still being made from summary statistics, static thresholds, and post-incident interpretation even though the published evidence shows that both source-water challenge and filtration performance are temporal, phase-dependent processes. In the Alberta case synthesised by Emelko and colleagues, burned and salvage-logged watersheds produced materially higher 95th-percentile turbidity and dissolved organic carbon than unburned reference watersheds — 15.3 NTU and 4.6 mg/L for burned, 18.8 NTU and 9.9 mg/L for salvage-logged, compared with 5.1 NTU and 3.8 mg/L in unburned reference catchments — changing chemical demand, solids loading, and treatment-train suitability (Emelko et al., 2011). The same paper documents design-threshold exceedance rates rising from approximately 2% (turbidity \>10 NTU) and 4% (DOC \>4 mg/L) in reference catchments to 16% and 48% respectively in salvage-logged catchments. In the process-performance work led by Schmidt, Anderson, and Emelko, average log-reduction was shown to overstate average treatment and understate risk: a process running at 4-log for 23 hours and 1-log for one hour has an arithmetic average of 3.875-log but an effective log-reduction of approximately 2.37-log, implying roughly 32 times more pathogen passage than the average metric suggests (Schmidt et al., 2020). In the late-in-cycle filtration work, removal deteriorated from approximately 5-log under stable conditions to approximately 3-log at end-of-run, 2.1-log in early breakthrough, and 1.4-log in late breakthrough — while effluent turbidity could still remain below 0.1 NTU (Emelko et al., 2003).

WaterOS is the proposed architectural response. It is not another dashboard layered on top of raw process data. It is a Scientific Operating System that enforces provenance-first verification, gate-ordered reasoning, longitudinal window logic, and deterministic non-claim rules. Its role is to transform wildfire disturbance, hydrologic regime, raw-water telemetry, coagulant-response signals, oxidation decisions, and filter-cycle phase information into executable governance. Rather than asking only whether a monthly mean or end-of-run report looks acceptable, WaterOS asks whether the current process trajectory remains coherent, whether the response path is traceable, and whether the system has enough contextual truth to make any claim at all.

The guiding ingestion principle is stated explicitly: 语境生义 (Yǔjìng shēng yì) — context creates meaning. In this domain, a turbidity value without filter age, particle context, hydrologic regime, dose history, oxidation mode, and recovery quality is not a control signal. It is only fossilised evidence. Likewise, a non-detect without recovery context is not proof of safety. WaterOS therefore treats the causal chain, not the isolated number, as the auditable object.

REV3 advances over REV2: recovery-context prohibition PC-06 now formally codified; effective log-reduction notation standardised to μ\_LR and σ²\_LR throughout; reserve state thresholds carry inline calibration caveats; placeholder Phase Jitter Index thresholds grounded in Emelko et al. (2003) filter-phase data; Agency Score gate checklist replaces the ambiguous weighted formula; Appendix B.0 provides a canonical variable dictionary for all synthetic metrics; Section 7.5 expanded into evidence-burden subsections per certification tier.

*Calibration caveat: REV3 defines the architecture, event grammar, gate order, and engineering formulas. It does not claim universally valid utility-specific thresholds for zeta potential, UV254 triggers, coagulant-dose bands, source-water tipping points, or local economic coefficients. Those remain pending calibration with the domain authority and plant partners.*


**1. Problem Definition**

This section names the distress precisely. The relevant literature does not say that treatment science is absent; it says the operating environment now contains disturbances whose significance is mediated by time, variability, and infrastructure flexibility. WaterOS therefore begins by defining the failure in its proper form: a latency and governance problem embedded inside a treatment problem.

**1.1 The Core Distress**

The core distress is the observability-to-action gap between a correctable shift in treatment trajectory and the institutional moment at which that shift becomes legible enough to change behaviour. After wildfire, source water can exhibit increased sediment loading, nutrients, metals, organics with higher humic content, altered pH and alkalinity, and greater treatment challenge; these changes translate directly into coagulant demand, oxidant demand, filter efficiency, solids production, and disinfection by-product pressure (Gifford, 2025; Emelko et al., 2011). The WRF 5168 piloting work documents the scale of this challenge: an ash mobilisation event from the Bolt Creek fire produced turbidity exceeding 103 NTU from a baseline of approximately 0.3 NTU, requiring alum dose escalation from approximately 6 mg/L under baseline conditions to 15–19 mg/L during the spiked event (Gifford, 2025). Post-fire coagulant demand escalation has been documented at 8–12 times baseline requirements under severe disturbance conditions, an increase that can convert a seven-day chemical supply into less than one day of operation without restocking (Emelko et al., 2011). These figures are drawn from specific documented scenarios and should be treated as illustrative order-of-magnitude anchors rather than universal plant constants; site-specific calibration will determine the applicable range for each deployment.

The disappearance of the reasoning chain occurs in three places. First, source-water disturbance is often described as a raw-water condition rather than as a treatment-governance condition. Second, treatment performance is frequently summarised using arithmetic means or static pass/fail thresholds even when the process is demonstrably variable over time. Third, uncertainty in pathogen-related enumeration and recovery can be silently collapsed into point estimates, allowing false confidence in low counts or non-detects. Emergency response costs at unprepared utilities have been documented at $2–10M CAD per event in the Canadian utility context; this figure is order-of-magnitude representative and requires utility-specific validation for formal business cases.

**1.2 Redefining the Product**

The old framing noun is treatment adequacy as a stored artifact. The new framing verb is treatment coherence as an enforced process. Under the old framing, a utility accumulates samples, reports, jar tests, pilot findings, and compliance metrics, then interprets them. Under the new framing, the system continuously evaluates whether current source-water state, treatment choices, and filter-phase behaviour remain inside an auditable envelope. The product is not a report about treatment. The product is the governance that keeps treatment truthful while the disturbance evolves.

| **Old framing noun** | **New framing verb** | **Practical consequence** |
| - | - | - |
| Compliance result | Trajectory governance | Moves attention from endpoint status to current phase and direction of motion |
| Average log-reduction | Effective phase-aware performance | Avoids understating risk when variability is high |
| Source-water protection | Source-water supply and protection with treatability | Includes cost, timing, infrastructure, and operating adaptability |
| Static monitoring threshold | Executable gate | Lets the same evidence drive deterministic response paths |

**1.3 Scope**

WaterOS is intentionally scoped around the governance seam between watershed disturbance and treatment response. That seam is where most institutions currently depend on fragmented ownership: watershed actors manage burn severity, weather, and source risk; plant operators manage chemistry and filtration; regulators and risk analysts see summarised artifacts later. The operating system exists to bridge that seam without pretending to replace any discipline.

The scope also includes what might be called counterfactual silence. A well-designed WaterOS deployment is valuable not only when it generates a recommendation, but also when it refuses to speak beyond the evidence. In a domain where averages, sparse sampling, and low-recovery enumeration can create false assurance, silence under insufficient basis is a core output rather than a system defect.

REV3 covers post-wildfire drinking-water treatability governance for surface-water systems using conventional or near-conventional treatment trains. Out of scope for REV3: utility-specific civil design, procurement packaging, detailed SCADA implementation, regulatory filing language, and any claim that WaterOS alone can mitigate catastrophic debris-flow events. The Burned Area Emergency Response (BAER) literature explicitly notes that some extreme post-fire impacts may not be reasonably mitigated through provider preparedness or response alone (WRF/CWN, 2014).


**2. Architectural Invariants**

WaterOS inherits the Sovereign Process Architecture invariants and re-expresses them in water-treatment language. This section defines the non-negotiable structure of trust in the system. These invariants are independent of any one utility, and they are the reason the platform can remain rigorous while still leaving plant-specific thresholds open for calibration.

**2.1 SPA as Trust Backbone**

| **Invariant** | **WaterOS meaning** | **Operational implication** |
| - | - | - |
| I. Provenance-First Verification | No concentration, status, or resilience claim is valid unless the system can show where the value came from, what sensor or method produced it, what transformation was applied, and what uncertainty or recovery caveat remains. | Every signal is signed to a source, time window, processing step, and confidence envelope before use. |
| II. Gate-Ordered Reasoning Hierarchy | The system does not let later-stage inferences override missing upstream context. Source-water state, process context, and measurement quality must be evaluated before recommendations or verdicts. | A recommendation can be withheld even when a single metric looks acceptable if upstream context is insufficient. |
| III. Longitudinal Window-Based Truth | Filter performance, post-fire response, and water quality are judged over natural windows rather than as isolated points. | Filter-cycle window and watershed-hydrology window are both first-class clocks. |
| IV. Deterministic Validator with Prohibited Content | Certain claims are architecturally blocked regardless of user pressure or superficial evidence. | The system may escalate to 'insufficient basis' instead of giving a falsely precise answer. |

**2.2 Existing Governance Framework as Executable Logic**

The domain already contains governance vocabulary, but much of it currently lives as scientific terminology or operator know-how rather than executable logic. WaterOS formalises five vocabularies in particular. The first is SWSP, the move beyond source-water protection toward a framing that includes quantity, timing, quality, and the act and cost of producing safe water (Emelko et al., 2011). The second is filter-cycle phase: ripening, stable operation, end-of-run, early breakthrough, and late breakthrough (Emelko et al., 2003). The third is treatment-train response logic such as the WRF 5168 operational tradeoff matrix (Gifford, 2025). The fourth is uncertainty-aware enumeration and risk interpretation, where low counts and averages cannot be interpreted without recovery and variability context (Schmidt et al., 2020; Emelko et al., 2010). The fifth is the Burned Area Emergency Response (BAER) framework, which defines the outer limit of provider-preparedness efficacy and informs what WaterOS must treat as beyond-governance territory (WRF/CWN, 2014).

**2.3 Logic Hierarchy**

WaterOS establishes a dominance order so the system knows what kind of truth may overrule what other kind. Foundation dominates all downstream interpretation because raw-water meaning depends on watershed state, hydrologic regime, and baseline plant context. Command then governs because the question is not only what the water looks like but whether the treatment train is being coordinated coherently. Reserves rank next because resilience depends on remaining manoeuvre room: alkalinity margin, coagulant range, alternate source availability, solids handling, and filter run flexibility. Growth and Entropy are the fastest-moving modules, often producing the most visible signals, but they are not allowed to speak alone.

**2.4 Context Creates Meaning**

The practical consequence is that WaterOS never stores a naked value as a first-class truth object. Each signal is wrapped in window identity, baseline relation, provenance quality, and process position. A turbidity value of 0.08 NTU means stable operation six hours after backwash and end-of-run at 48 hours. A DOC of 5 mg/L means normal variability during spring freshet and post-fire degradation during baseflow. A non-detect from a method with 20% recovery implies possible concentrations up to approximately five times the detection limit; the same result from a 95% recovery method is substantially stronger evidence of absence. WaterOS encodes these context distinctions as first-class data structures, not metadata.

**3. System Overview**

This section describes the architecture as a layered operating system. The objective is to preserve causal order from source signal to human action, not to maximise software complexity. WaterOS is built to degrade gracefully: if higher-signal sensors such as zeta potential or UV254 are unavailable, the system can continue to function in a constrained mode while explicitly reducing the confidence of its recommendations.

**3.1 Layered Architecture**

| **Layer** | **Name** | **Function** |
| - | - | - |
| A | Source Event Layer | Ingest wildfire, rainfall, runoff, ash, watershed, alternate-source, and hydrologic regime events. |
| B | Raw-Water Telemetry Layer | Collect turbidity, particle counts, pH, alkalinity, DOC/TOC, UV254, metals flags, temperature, flow, and related process inputs. |
| C | Treatment-State Layer | Track oxidation mode, coagulant chemistry, dose history, flocculation conditions, settled-water indicators, filtration rate, and filter runtime. |
| D | Window Engine Layer | Maintain dual windows for filter-cycle phase and watershed post-disturbance regime; align time-indexed evidence to the correct clock. |
| E | Process Physics Layer | Compute latent state estimates such as reserve stress, growth pressure, entropy leakage, phase jitter, and coherence collapse risk. |
| F | Validator & Governance Layer | Apply V0–V7 defense-in-depth logic, prohibited-content rules, agency gate checklist, and runbook routing. |
| G | Interface & Evidence Layer | Present auditable recommendations, escalation states, replay trails, and certification views to operators, scientists, and auditors. |

**3.2 Minimal Viable Sensor Envelope with Graceful Degradation**

The literature does not provide a single universal instrumentation prescription, but it identifies signals with unusually high operational relevance. The 2025 WRF 5168 presentation highlights zeta potential, particle counts, and UV254 monitoring as valuable resilience inputs. WaterOS uses these findings to define a three-tier envelope.

| **Envelope tier** | **Required inputs** | **WaterOS mode** | **Lost capability if absent** |
| - | - | - | - |
| Tier A — Full observability | Raw-water turbidity, particle counts, pH, alkalinity, UV254, DOC/TOC, zeta potential, flow, coagulant dose history, oxidation status, filter runtime, settled-water indicators | Full route selection and confidence-weighted recommendation | None |
| Tier B — Operational observability | Raw-water turbidity, pH, flow, coagulant dose history, filter runtime, particle counts or settled-water turbidity surrogate, oxidation status | Deterministic route selection with reduced diagnostic specificity | Reduced capacity to discriminate organic vs charge-driven shifts |
| Tier C — Survival observability | Raw-water turbidity, flow, filter runtime, operator-entered chemical changes, advisory events | Escalation-first mode; system prioritises caution and logging over fine optimisation | Cannot support strong optimisation or certification claims |

*Engineering caveat: exact minimum input sets depend on treatment-train design and what variables are already trusted in operations. REV3 defines a principled minimum envelope, not a universal procurement list.*

**3.3 Data Products**

WaterOS prefers evidence packages over single numbers. Each data product is a compact container of explanation, not a decorative KPI. The same Flight Recorder serves a minute-by-minute operator view, a campaign view for scientists, and a certification packet for management without maintaining separate truths.

| **Data product** | **Primary user** | **Decision enabled** |
| - | - | - |
| State Vector | Operators and scientists | What phase are we in, and what module is dominating? |
| Runbook Recommendation | Operators | Which intervention path should be taken now? |
| Flight Recorder Ledger | Auditors and domain authority | Why did the system say or suppress what it did? |
| Calibration Backlog | Scientists and implementation leads | Which thresholds remain uncalibrated and what evidence is needed? |
| Certification Packet | Utilities, partners, regulators | What level of process governance is verifiably in place? |


**4. Event Model**

A Scientific Operating System lives or dies on its event grammar. WaterOS uses events rather than loose observations so that every important transition can be time-stamped, attributed, validated, and replayed. The event model is intentionally compact: broad enough to represent the domain, narrow enough to be implementable without semantic drift. The Recovery Update event type (E10) was introduced in REV2 and is retained here; it closes the provenance gap for analytical recovery data, which cannot remain informal in a system that enforces Invariant I.

**4.1 Canonical Event Categories**

| **Event category** | **Meaning** | **Typical triggers** |
| - | - | - |
| E1. Disturbance Declaration | An exogenous event has changed likely source-water behaviour. | Wildfire perimeter update, ash deposition notice, major rainfall over burned area |
| E2. Hydrologic Regime Shift | The watershed has moved into a different water-quality window. | Stormflow onset, hydrograph recession, snowmelt freshet |
| E3. Raw-Water Excursion | A monitored source variable leaves expected bounds or changes too quickly. | Turbidity, UV254, particle count, pH, alkalinity, DOC shift |
| E4. Treatment Adjustment | The plant changes a controllable treatment parameter. | Coagulant dose change, chemical switch, oxidant change, filtration-rate change |
| E5. Filter Phase Transition | The filter state crosses a phase boundary. | Runtime, particle jitter, effluent turbidity pattern; covers ripening-to-stable, stable-to-end-of-run, and breakthrough transitions |
| E6. Reserve Compression | System maneuvering capacity shrinks. | Alkalinity depletion, solids overload, limited backup source, chemical range compression |
| E7. Entropy Leak | Evidence suggests rising passage, DBP burden, taste/odour burden, or unresolved loss. | Breakthrough indicators, DBP concern, particle passage |
| E8. Governance Escalation | The system lacks sufficient basis for a strong claim or the operator must be looped in. | Missing provenance, prohibited content hit, uncertainty overflow |
| E9. Advisory / Compliance Interface | An event has external reporting significance. | Boil-water advisory, regulatory notification, audit package generation |
| E10. Recovery Update | New analytical recovery data from the laboratory is available for pathogen or surrogate enumeration. | USEPA 1623 or equivalent lab result ingested; updates the recovery distribution used in risk calculations |

**4.2 Schema Summary**

Every WaterOS event shares a common schema envelope with six mandatory blocks: identity, provenance, timing, context, evidence, and governance. The Recovery Update event carries an additional recovery\_distribution block containing the distributional parameters required for Bayesian integration.

| **Schema block** | **Required fields** |
| - | - |
| Identity | event\_id, event\_type, plant\_id, source\_system |
| Provenance | origin, acquisition\_method, validator\_stage, source\_signature, quality\_flag |
| Timing | observed\_at, ingested\_at, window\_id, filter\_age\_minutes or hydrologic\_window\_ref |
| Context | watershed\_state, oxidation\_mode, coagulant\_mode, runtime\_context, disturbance\_context |
| Evidence | metric\_name(s), value(s), units, uncertainty or recovery metadata, delta\_from\_baseline |
| Governance | route\_taken, prohibited\_claim\_hits, escalation\_state, agency\_gate\_status, operator\_ack |

**4.3 Example Payload**

The following payload illustrates a raw-water excursion with filter-phase context. Values are architectural illustrations, not universal thresholds.

\{  
  "event\_id": "evt\_20260307\_001942",  
  "event\_type": "E3\_RAW\_WATER\_EXCURSION",  
  "plant\_id": "pilot\_portland\_like\_001",  
  "origin": "online\_sensor",  
  "source\_signature": "sha256:rawwater-sonde-7c1d...",  
  "observed\_at": "2026-03-07T14:19:42Z",  
  "window\_id": "hydro\_recession\_wk02",  
  "context": \{  
    "disturbance\_context": "post\_wildfire\_recession",  
    "oxidation\_mode": "pre\_ozone",  
    "coagulant\_mode": "alum",  
    "filter\_runtime\_minutes": 1280  
  \},  
  "evidence": \{  
    "raw\_turbidity\_ntu": 18.4,  
    "uv254\_cm\_inv": 0.162,  
    "particle\_count\_per\_ml": 19,  
    "zeta\_potential\_mv": -4.2,  
    "delta\_vs\_7d\_baseline": \{ "raw\_turbidity\_ntu": 8.9 \},  
    "uncertainty\_class": "instrument\_verified"  
  \},  
  "governance": \{  
    "route\_taken": "PATH\_A\_TREATMENT\_OPTIMISATION",  
    "prohibited\_claim\_hits": \[\],  
    "agency\_gate\_status": "all\_gates\_passed",  
    "escalation\_state": "operator\_review\_required"  
  \}  
\}


**5. Process Modules**

The five modules correspond to distinct control questions: what is accelerating, what buffer remains, what is coordinating the system, what baseline governs interpretation, and what loss is already leaking through. Together they allow WaterOS to describe treatment distress as a governed ecology rather than a pile of disconnected alarms.

**5.1 Growth / Momentum Module**

The Growth module measures the rate and direction at which treatment challenge is accumulating. Growth does not mean health; it means the momentum of source-water and treatability stress that pushes the plant toward harder operating conditions. Emelko et al. (2011) documented persistent elevation of 95th-percentile turbidity from 5.1 NTU in unburned catchments to 15.3 NTU in burned and 18.8 NTU in salvage-logged watersheds, with corresponding DOC elevations from 3.8 to 4.6 to 9.9 mg/L. Design-threshold exceedance rates for turbidity above 10 NTU rose from approximately 2% in reference catchments to 11% in burned and 16% in salvage-logged watersheds; for DOC above 4 mg/L, from approximately 4% to 9% to 48% respectively. These numbers show that post-fire Growth pressure is not a transient spike but a sustained shift in the treatment challenge distribution.

| **Element** | **Definition** |
| - | - |
| Goal | Detect accelerating raw-water challenge before it manifests as downstream treatment failure. |
| Inputs | Raw-water turbidity, UV254, DOC/TOC, particle counts, pH, alkalinity shifts, storm/hydrologic context, wildfire disturbance metadata. |
| Core metrics | Growth Pressure Index (GPI), rate-of-change for turbidity and UV254, design-threshold exceedance duration, challenge composition class. |
| State classification | Dormant / Rising / Accelerating / Acute |
| Rationale | Emelko et al. (2011) showed persistent elevation in turbidity and DOC after wildfire and salvage logging; Gifford (2025) details changed chemical demand and organics burden in WRF 5168 piloting work. |

*Engineering estimate: GPI is a synthetic architecture variable. Its formula and coefficient definitions are in Appendix B; values are pending calibration against utility-specific telemetry. See Appendix E for dataset requirements.*

**5.2 Reserve / Stability Module**

The Reserve module tracks remaining manoeuvre room. A plant may appear acceptable at one instant while its operating margin is collapsing under the surface. Reserve depletion in post-wildfire scenarios is especially rapid because coagulant demand can increase by a documented factor of 8–12 times baseline in severe disturbance conditions: a plant operating at approximately 5 mg/L PACl under reference conditions may require 40–60 mg/L during an impacted event (Emelko et al., 2011). This translates a seven-day chemical supply into less than one day without restocking. These figures represent documented operational examples from the published literature and should be treated as order-of-magnitude anchors; site-specific calibration will determine the applicable range for each plant. The 2025 resilience piloting work identifies the key response levers: broader coagulation dose ranges, supplemental alkalinity or acid, coagulant aid, higher solids-handling capacity, slower filtration rates, and alternate or protected sources (Gifford, 2025).

| **Element** | **Definition** |
| - | - |
| Goal | Measure how much response flexibility remains before the plant becomes brittle. |
| Inputs | Alkalinity, settled-water pH, chemical inventory range, sludge/solids burden, filtration rate, backup-source status, operator staffing or acknowledgements. |
| Core metrics | Reserve Integrity Score (RIS), alkalinity margin, coagulant-flexibility index, alternate-source readiness, filter productivity margin. |
| State classification | Buffered (\>48 h remaining capacity; \>7 days chemical stock) / Narrowing (24–48 h; 3–7 days) / Compressed (12–24 h; 1–3 days) / Exhausted (\<12 h; \<1 day). These thresholds are engineering estimates pending site-specific calibration; see Section 11.2 and Appendix E for calibration requirements. |
| Rationale | The 2025 tradeoff framework shows resilience depends on retained ability to alter chemistry and hydraulics without breaking other objectives; coagulant demand scaling from Emelko et al. (2011). |

**5.3 Coherence / Command Module (Xin — Primary Gate)**

The Command module is the coordinating brain of WaterOS and its primary gate. It evaluates whether the observed actions taken by the plant match the challenge that is present. The WRF 5168 operational tradeoff matrix provides a practical template for executable response logic: low settled-water pH, depleted alkalinity, excessive solids production, insufficient turbidity removal, insufficient TOC removal, and DBP formation do not respond to the same interventions in the same way (Gifford, 2025). The zeta-potential operating range of approximately −3 mV to +1 mV for optimal coagulation (Gifford, 2025) provides a measurable coherence anchor; departure from this range under rising influent challenge represents a quantifiable coherence deficit that feeds the Coherence Alignment Score.

| **Element** | **Definition** |
| - | - |
| Goal | Judge whether treatment actions remain coherent with the current challenge profile. |
| Inputs | Current challenge class, current coagulant and oxidant choices, dose history, zeta potential, operator interventions, route history, prohibited-content hits. |
| Core metrics | Coherence Alignment Score (CAS), intervention fit, route confidence, override burden, zeta-potential proximity to −3 to +1 mV target range. |
| State classification | Aligned / Debating / Mismatched / Disordered |
| Rationale | The domain contains enough codeable tradeoff logic to make response-path audit possible, especially where zeta potential, particle counts, and UV254 are present (Gifford, 2025). |

*Engineering caveat: the zeta-potential target range of −3 to +1 mV is drawn from the WRF 5168 operational material (Gifford, 2025) and represents an anchor for coagulation optimisation. Utility-specific ranges require calibration against local water chemistry.*

**5.4 Foundation / Anchor / Normalisation Module**

Foundation determines what the current evidence should mean in this plant, in this watershed, in this season, under this disturbance history. Without normalisation, an elevated value can be misread as extraordinary when it is expected for the current hydrologic window, or as acceptable when it exceeds the baseline desired water quality for that plant. The 2011 paper is explicit that treatment implications vary with hydroclimatic conditions and that exceedance behaviour against design thresholds is more informative than isolated values (Emelko et al., 2011).

| **Element** | **Definition** |
| - | - |
| Goal | Normalise every decision against basin history, season, disturbance phase, and plant design context. |
| Inputs | Historical baseline windows, desired source-water envelope, design thresholds, watershed condition, season, treatment-train capabilities. |
| Core metrics | Context Sufficiency Index (CSI), threshold exceedance time fraction, baseline displacement, disturbance-window class. |
| State classification | Anchored / Shifted / De-anchored / Unknown |
| Rationale | Without foundation, the same number can mean normal recession behaviour or a new acute challenge; WaterOS must know the difference (Emelko et al., 2011; WRF/CWN, 2014). |

**5.5 Entropy / Dissipation Module**

Entropy measures loss. In water treatment, loss appears as rising pathogen passage risk, breakthrough, DBP pressure, taste and odour burden, increasing solids production, reduced filter efficiency, or any form of deterioration that leaks process integrity. The 2003 filtration study is decisive: at optimised stable conditions the pilot filter achieved approximately 5-log C. parvum removal, yet end-of-run operation with effluent turbidity still below 0.1 NTU deteriorated to approximately 3-log, early breakthrough to approximately 2.1-log, and late breakthrough to approximately 1.4-log (Emelko et al., 2003). Mean analytical recovery for C. parvum oocysts and microspheres in that study was approximately 75%. The 2020 paper then demonstrates why averages conceal that leakage: a process running at 4-log for 23 hours and 1-log for one hour reports an arithmetic average of 3.875-log but an effective log-reduction (μ\_LR − ½·ln(10)·σ²\_LR) of approximately 2.37-log, implying roughly 32 times more pathogen passage than the average metric claims (Schmidt et al., 2020).

The module is deliberately cross-objective. Entropy is not only pathogen passage. It includes DBP burden, organic carryover, productivity loss, sludge and solids penalties, taste and odour deterioration, and claim inflation. A plant can win one lane while losing the system. WaterOS must see the whole leak pattern.

| **Element** | **Definition** |
| - | - |
| Goal | Quantify where treatment truth is already leaking out of the system. |
| Inputs | Filter-phase evidence, particle counts, effluent turbidity behaviour, effective log-reduction model, DBP risk context, taste/odour burden, solids output, analytical recovery distribution. |
| Core metrics | Entropy Leak Score (ELS), phase-jitter burden, effective-passage risk (computed using μ\_LR and σ²\_LR per Appendix B.1), DBP pressure class. |
| State classification | Contained / Seeping / Leaking / Breach |
| Rationale | Entropy is the module most likely to force governance escalation because it is where false reassurance becomes dangerous (Emelko et al., 2003; Schmidt et al., 2020). |


**6. Process Physics**

WaterOS requires a domain-specific equilibrium model to distinguish ordinary fluctuation from structural drift. In this deployment, the conserved tension is between Treatability Capacity and Disturbance Load. Capacity is the plant's presently available ability to convert affected source water into safe potable water at reasonable cost and continuity. Disturbance Load is the burden imposed by wildfire legacies, hydrologic transport, organic/particulate challenge, and process variability. Coherence exists while capacity exceeds load with sufficient reserve. Distress appears when load rises faster than command and reserve can respond.

**6.1 The Conserved Tension**

Treatability Margin (TM) = Capacity(t) - DisturbanceLoad(t)  
  
Capacity(t) =  
  w1 \* ReserveIntegrityScore  
+ w2 \* CoherenceAlignmentScore  
+ w3 \* FilterProductivityMargin  
+ w4 \* AlternateSourceReadiness  
+ w5 \* OperatorResponseReadiness  
  
DisturbanceLoad(t) =  
  v1 \* GrowthPressureIndex  
+ v2 \* ThresholdExceedanceBurden  
+ v3 \* PhaseJitterIndex  
+ v4 \* OrganicChallengeIndex  
+ v5 \* SolidsAndDBPStress

*All coefficients w1–w5 and v1–v5 are engineering placeholders pending calibration. REV3 claims only that this decomposition is necessary and structurally faithful to the published logic. Variable definitions for every term are in Appendix B.0.*

**6.2 Early Warning Mathematics**

The WaterOS early-warning problem is to detect when the process is still outwardly functional but its trajectory has bent toward failure. The 2003 filtration evidence shows that end-of-run deterioration can begin while turbidity remains below thresholds commonly used to narrate acceptable performance. WaterOS names this latent destabilisation phase Phase Jitter. It is not one sensor; it is the discordance among sensors, runtime, and response fit.

PhaseJitterIndex (PJI) =  
  a1 \* abs(d(raw\_turbidity)/dt)\_norm  
+ a2 \* abs(d(particle\_count)/dt)\_norm  
+ a3 \* abs(d(uv254)/dt)\_norm  
+ a4 \* FilterAgePressure  
+ a5 \* ResponseMismatchPenalty  
+ a6 \* RecoveryUncertaintyPenalty  
- a7 \* ContextSufficiencyIndex  
  
If PJI \> theta\_1 and EntropyLeakScore is rising:  
  state = "Early Drift"  
If PJI \> theta\_2 and filter phase = end\_of\_run or early\_breakthrough:  
  state = "Terminal Approach"

Placeholder threshold ranges, grounded in the Emelko et al. (2003) filter-phase evidence and intended as starting points for calibration discussion, not universal operational values:

theta\_1 (Early Drift): a normalised turbidity slope exceeding approximately 0.1 h⁻¹ sustained over a 30-minute window, or equivalently an effluent turbidity trend rising at more than approximately 0.005 NTU/h over a 2-hour window. The 2003 study observed meaningful deterioration in log-removal while effluent turbidity remained below 0.1 NTU, implying that end-of-run drift begins before the turbidity signal becomes obvious.

theta\_2 (Terminal Approach): effluent turbidity between 0.1 and 0.3 NTU with positive acceleration, or PJI composite score exceeding approximately 0.3 on the normalised index. This range corresponds to the early-breakthrough phase defined in Emelko et al. (2003), where log-removal had already deteriorated to approximately 2.1-log.

*Engineering caveat: theta\_1 and theta\_2 are placeholder ranges derived from published phase definitions. They must be fitted using historical filter-cycle records and piloting data at each utility. WaterOS does not claim a universal early-warning cutpoint.*

**6.3 Stability Classification**

| **Stability class** | **Logical condition** | **Operational meaning** |
| - | - | - |
| S0 Stable | TM comfortably positive; PJI below theta\_1; ELS Contained | Current route is coherent and reserves remain available. |
| S1 Strained | TM positive but narrowing; PJI approaching theta\_1 | Plant remains in control but needs closer watch or pre-emptive adjustment. |
| S2 Precarious | TM near zero; PJI above theta\_1; reserve Compressed | Plant is one disturbance step away from a disorder state. |
| S3 Failing | TM negative or PJI above theta\_2 or ELS Breach | Immediate escalation, restrictive claims, and active intervention are required. |


**7. Sovereign Governance Layer**

The Sovereign Governance Layer is where WaterOS ceases to be an analytics package and becomes an auditable system of action. Every claim is either supported, suppressed, or escalated through a fixed sequence. The purpose of this layer is not to remove human agency; it is to prevent untraceable agency from masquerading as evidence.

**7.1 Flight Recorder**

The Flight Recorder is the immutable ledger that captures source inputs, transformations, route decisions, suppressed claims, model versions, and human acknowledgements. A scientist or operator must be able to replay exactly why the system concluded that coagulant under-dose was likely, why it escalated a reserve compression alert, or why it refused to affirm adequacy.

| **Ledger field group** | **Contents** |
| - | - |
| Signal provenance | Sensor/source identifier, acquisition time, quality flag, signature hash |
| Context provenance | Window assignment, filter phase, disturbance regime, baseline version |
| Derived state | Module scores, PJI, TM, ELS, CAS, RIS, CSI |
| Governance outcome | Route taken, recommendation, prohibitions triggered, agency gate status, escalation |
| Human interaction | Acknowledgement, override reason, comment, time of action |

**7.2 Defense-in-Depth Pipeline (V0–V7)**

| **Node** | **Name** | **Function** |
| - | - | - |
| V0 | Source Authenticity | Reject spoofed, stale, or unprovenanced inputs. |
| V1 | Schema & Unit Integrity | Verify types, units, ranges, and timestamp coherence. |
| V2 | Window Assignment | Bind each event to correct filter-cycle and watershed windows. |
| V3 | Context Sufficiency | Check whether enough baseline and process context exists for interpretation. |
| V4 | Process Physics | Compute module scores, PJI, TM, ELS, and challenge class. |
| V5 | Route Logic | Select runbook path using tradeoff matrix and plant mode. |
| V6 | Prohibited Content | Suppress or downgrade claims that violate invariant rules. |
| V7 | Attestation | Sign output to Flight Recorder with model/version metadata and human acknowledgement status. |

**7.3 Socratic Handshake Gate + Agency Gate Checklist**

The Socratic Handshake Gate is WaterOS's final self-questioning step before release. The system must answer five questions affirmatively before it may emit a strong operational claim: Do I know what changed? Do I know in which window it changed? Do I know whether the current response path matches the present challenge? Do I know whether uncertainty or recovery defects undermine the evidence? Do I know whether the claim is prohibited or overreaching?

The Agency Gate Checklist replaces the weighted score formula used in REV2. The weighted formula produced ambiguous results because positive weights summed to a value above 1.0 before penalties, making the net score difficult to audit. The checklist below is gate-ordered and binary: all conditions must pass before a strong claim or Tier 3 certification may be issued. A single failed gate blocks the claim and requires escalation.

| **Gate** | **Condition required** | **If failed** |
| - | - | - |
| AG-1 | ContextSufficiencyIndex ≥ 0.80 (baseline and process context sufficient) | Suppress claim; escalate to scientist review |
| AG-2 | ProvenanceIntegrity = verified (all signals signed and traceable) | Demote to advisory mode; flag missing provenance in ledger |
| AG-3 | RouteConfidence ≥ 0.70 (runbook path matched to challenge class) | Block optimisation claim; log route uncertainty |
| AG-4 | ReserveIntegrityScore ≥ Narrowing (manoeuvre room still exists) | Issue reserve compression alert; restrict forward claims |
| AG-5 | RecoveryRigor = recovery distribution known and applied (not assumed default) | Block pathogen-risk claims; apply uncertainty escalation |
| AG-6 | OperatorAcknowledgement received for current phase transition (if any) | Hold attestation; notify operator to acknowledge |
| AG-7 | No ProhibitedContent violation in candidate claim | Suppress claim; log violation type in Flight Recorder |
| AG-8 | UncertaintyOverflow = false (bounds computable and within tolerance) | Escalate to insufficient-basis state; refuse certification |

*The Agency Gate Checklist governs how forcefully WaterOS may speak. It does not replace scientific measurement. A system that passes all eight gates may still be operating with uncalibrated coefficients; those limitations are disclosed in the Calibration Backlog.*

**7.4 Prohibited Content Rules**

The following claims are architecturally blocked in every WaterOS deployment regardless of input data. Six rules are now defined.

PC-01: WaterOS must not claim treatment adequacy from arithmetic average log-reduction alone. The 2020 paper explicitly demonstrates that μ\_LR systematically overstates performance: a process at 4-log for 23 hours and 1-log for one hour reports μ\_LR = 3.875 but an effective log-reduction of approximately 2.37, implying approximately 32 times more pathogen passage (Schmidt et al., 2020).

PC-02: WaterOS must not claim pathogen security from effluent turbidity compliance alone. The 2003 filtration evidence shows deterioration to approximately 3-log at end-of-run, 2.1-log at early breakthrough, and 1.4-log at late breakthrough — while turbidity may still remain below 0.1 NTU (Emelko et al., 2003).

PC-03: WaterOS must not claim that source-water protection alone fully addresses wildfire treatability. The 2011 paper argues for SWSP because treatment cost, timing, infrastructure selection, and adaptability matter (Emelko et al., 2011).

PC-04: WaterOS must not assert that land-management or provider-response efficacy is already settled where the 2014 workshop record explicitly notes lack of framework or consensus for evaluating avoided treatment impacts (WRF/CWN, 2014).

PC-05: WaterOS must not imply that a non-detect equals zero risk when analytical recovery is below approximately 75% or when sample volume is insufficient for confident inference (Emelko et al., 2010).

PC-06: WaterOS must not claim "no pathogens" or "safe" from any non-detect sample unless the recovery-adjusted upper 95% credible interval falls below the applicable risk threshold. A non-detect from a method with 20% recovery implies possible concentrations up to approximately five times the detection limit; the recovery distribution (alpha, beta parameters) must be explicitly ingested as an E10 Recovery Update event before any risk-related claim may be issued. Grounding: Emelko et al. (2010) — non-detects are meaningless without recovery context.

**7.5 Certification Tier Structure**

Certification in WaterOS is framed around process-governance maturity, not regulatory substitution. A certified plant is not being declared universally safe by software. It is being recognised as having jointly calibrated, auditable governance for the way it interprets and responds to disturbance.

| **Tier** | **Meaning** | **Minimum condition** |
| - | - | - |
| Tier 1 — Observable | Plant has structured telemetry and provenance-first logging. | V0–V3 operational; minimum survival observability achieved. |
| Tier 2 — Governed | Plant can execute deterministic route logic with auditable suppressions and escalations. | V0–V6 operational; dual windows implemented; runbook active. |
| Tier 3 — Calibrated | Plant-specific thresholds and coefficients have been jointly calibrated with domain authority or scientific partners. | V0–V7 operational; empirical calibration package accepted; Agency Gate Checklist passes consistently. |

**7.5.1 Tier 1 Evidence Burden**

A plant seeking Tier 1 recognition must demonstrate: (a) minimum Tier C sensor envelope operational and providing time-stamped, typed, signed events to the Flight Recorder; (b) V0 through V3 validator nodes active and producing ledger frames; (c) at least one baseline window established for the primary process variables; (d) provenance fields populated for all ingested signals (manual-entry fields require explicit operator attribution); and (e) a documented gap policy specifying which signals may be absent and for how long before the system escalates to Survival observability mode.

**7.5.2 Tier 2 Evidence Burden**

A plant seeking Tier 2 recognition must satisfy all Tier 1 conditions and additionally demonstrate: (a) both filter-cycle and watershed hydrologic windows implemented and active in the Window Engine; (b) all six prohibited-content rules (PC-01 through PC-06) enforced at V6 with suppression events logged in the Flight Recorder; (c) runbook path coverage for all nine Governance Gate conditions in Section 9.1; (d) override logging active with mandatory reason codes; (e) suppressed-claim logging active with violation type classification; (f) replay capability demonstrated against at least three historical scenarios; and (g) minimum 90-day shadow or advisory mode operation prior to Tier 2 claim.

**7.5.3 Tier 3 Evidence Burden**

A plant seeking Tier 3 recognition must satisfy all Tier 1 and Tier 2 conditions and additionally demonstrate: (a) a signed coefficient registry produced through the Stage A through Stage D calibration programme defined in Appendix E; (b) site-specific threshold calibration report for theta\_1, theta\_2, and reserve state time thresholds reviewed and accepted by the domain authority; (c) Appendix D Validation Matrix passed at 100% for all test vectors TV-01 through TV-07 against the calibrated system; (d) Agency Gate Checklist (Section 7.3) passing consistently over a minimum 30-day live-assisted period; (e) scientist sign-off from the domain authority confirming calibration acceptability; (f) drift monitoring protocol in place specifying recertification triggers; and (g) model/version frozen for the certification window duration. Tier 3 lapses if any calibrated coefficient is changed without repeating Stage D sign-off.


**8. Operating Protocol / Runbook**

The runbook converts architecture into operator-facing time discipline. WaterOS tracks two coordinated timelines: a watershed timeline describing when source-water meaning changes, and a plant timeline describing when filtration and treatment dynamics enter vulnerable phases. Post-fire recession conditions typically influence water quality for 3–7 weeks; snowmelt freshet windows run 8–10 weeks; longer-duration legacy wildfire impacts may persist for years and require dedicated baseline recalibration (Emelko et al., 2011; WRF/CWN, 2014).

**8.1 Phase Timeline Table**

| **Window** | **Phase** | **Typical duration** | **Typical evidence** | **WaterOS posture** |
| - | - | - | - | - |
| Watershed | Disturbance Declaration | Days to weeks | Wildfire, burn perimeter, ash risk, rainfall forecast | Elevate source-context sensitivity; begin intensified monitoring. |
| Watershed | Acute Transport | Days | Stormflow, ash/sediment pulses, raw-water excursions | Rapid observability, dynamic route evaluation, source/backup strategy review. |
| Watershed | Recession | 3–7 weeks (post-fire) | Persistent but declining turbidity/DOC burden | Track exceedance duration, reserve depletion, and delayed organic burden. |
| Watershed | Snowmelt Freshet | 8–10 weeks | Spring turbidity and DOC surge | Activate Growth and Reserve monitoring; treat as elevated-challenge window. |
| Watershed | Legacy Regime | Months to years | Multi-season altered baseline and recurring threshold exceedance | Update baseline envelope; reconsider certification and calibration priorities. |
| Plant | Ripening | Minutes to ~2 hours | High particle count, variable removal post-backwash | Monitor only; no strong claims during unstable post-backwash period. |
| Plant | Stable Operation | Hours to days | Consistent low turbidity and particle pattern (~0.05 NTU) | Normal governance with trend monitoring. |
| Plant | End-of-Run Drift | Hours | Turbidity trending toward 0.1 NTU; PJI approaching theta\_1 | Pre-emptive optimisation; strengthen caution over claims. |
| Plant | Early Breakthrough | Hours | Turbidity 0.1–0.3 NTU; PJI above theta\_2 | Escalation and intervention; entropy leakage presumed credible. |
| Plant | Late Breakthrough | Minutes to hours | Turbidity \>0.3 NTU or equivalent breach pattern | Restrictive claims only; immediate corrective action and replay logging. |

**8.2 Economic Rationale**

The economic case for WaterOS does not depend on one dramatic cost anecdote. It follows from the systematic gap between when challenge becomes correctable and when institutions typically become confident enough to act. Emergency response costs at unprepared utilities have been referenced at $2–10M CAD per event in the Canadian utility context; this is an order-of-magnitude figure requiring utility-specific validation for formal business cases. The observability gaps below each have a concrete architectural correction.

| **Observability gap** | **What is delayed today** | **Economic effect** | **WaterOS correction** |
| - | - | - | - |
| Source-water meaning gap | Recognition that raw-water change has crossed from nuisance to treatability stress | Late chemical changes, over/under-dosing, missed alternate-source opportunities | Continuous windowed context and route triggers |
| Filter-phase gap | Recognition that a filter is drifting before obvious breach | Shortened runs, increased passage risk, downstream instability | PJI and phase-aware gate logic |
| Uncertainty gap | Recognition that non-detect or low counts are not evidence of safety | False reassurance, wrong risk posture, poor investment decisions | Recovery-aware provenance and prohibited content rules (PC-05, PC-06) |
| Tradeoff gap | Recognition that one intervention fixes one problem but worsens another | Increased DBPs, solids, alkalinity depletion, or lost productivity | Deterministic tradeoff matrix in Command module |


**9. Governance Gates as Executable Logic**

The domain already contains enough operational logic to justify an executable gate set. WaterOS converts that logic into transparent, inspectable tables so every response path can be reviewed by the domain authority and adapted to plant realities without collapsing into black-box behaviour.

**9.1 Existing Framework Gates Table**

| **Challenge condition** | **Primary route** | **Secondary caution** | **Example basis** |
| - | - | - | - |
| Low settled-water pH | Consider adding alkalinity; evaluate ferric switch where appropriate | Avoid assuming stronger coagulation is always beneficial | WRF 5168 operational tradeoff matrix |
| Depleted alkalinity | Add alkalinity; review ferric implications | Track reserve compression | WRF 5168 operational tradeoff matrix |
| Excessive solids production | Review over-dosing and ferric burden; consider process simplification | Do not treat turbidity removal as the only objective | WRF 5168 operational tradeoff matrix |
| Insufficient turbidity removal | Evaluate increased dose, alkalinity support, and/or switch to PACl or ozone-supported route | Track DBP and reserve tradeoffs | WRF 5168 operational tradeoff matrix |
| Insufficient TOC removal | Review dose, alkalinity, and oxidant strategy; pre-ozone may improve organics outcome | Watch for productivity impacts | WRF 5168 piloting summary |
| DBP formation pressure | Avoid reflexive pre-chlorine escalation; consider pre-ozone or other route changes | Do not worsen organics burden while chasing another variable | WRF 5168 piloting summary |
| Zeta potential outside −3 to +1 mV | Adjust coagulant dose or switch coagulant type; verify influent pH and alkalinity | Do not interpret zeta potential without concurrent turbidity and particle context | Gifford (2025) WRF 5168 |

**9.2 Ecosystem / Process Scoreboard**

| **Scoreboard lane** | **Green meaning** | **Amber meaning** | **Red meaning** |
| - | - | - | - |
| Growth | Challenge stable or declining | Challenge rising | Challenge accelerating or acute |
| Reserve | Adequate manoeuvre room (Buffered) | Margin narrowing (Narrowing or Compressed) | Buffer compressed or exhausted |
| Command | Response path aligned | Partial mismatch or low confidence | Response path incoherent or absent |
| Foundation | Context adequate and anchored | Baseline shifting | Unknown or de-anchored context |
| Entropy | Loss contained | Leakage emerging or seeping | Breach or credible passage risk |


**10. Implementation Architecture**

This section describes how WaterOS can be deployed without forcing an all-at-once digital transformation. The architecture is intentionally modular so that utilities can begin with a narrow operational pilot and grow toward a calibrated production deployment. Full expansion of this section — covering SCADA historian handshake, LIMS ingestion protocols, OT/IT boundary discipline, operator alarm semantics, and deployment-mode transitions — is planned for REV4 following initial domain authority engagement.

**10.1 Plant Data Sources**

At the edge, WaterOS consumes existing plant and field data rather than demanding wholesale hardware replacement. Online sensors, SCADA streams, laboratory data exports, operator-entered changes, watershed alerts, and pilot data can all be bound into the same event model. The data source map covers intake telemetry, rapid mix and coagulation-flocculation signals, settled-water indicators, filter-level telemetry, clearwell and finished-water signals, watershed advisory feeds, laboratory uploads, and operator action logs. The edge requirement is not uniform hardware; it is source authenticity, time integrity, and typed units.

**10.2 Connectivity and Offline Operation**

Because wildfire response can coincide with disrupted connectivity, WaterOS must support offline-first buffering. Edge nodes must continue local event capture, window assignment, and limited route logic during temporary outages, then replay to the cloud ledger once connectivity returns. Any offline recommendation must be visibly marked as such and re-attested after synchronisation. Sequence preservation and duplicate suppression are mandatory replay requirements.

**10.3 Cloud / Governance Services**

The cloud tier hosts the event ledger, calibration registry, window engine, governance pipeline, evidence replay, and certification packet generator. Separation between immutable evidence and mutable calibration artifacts is essential: future recalibration must not rewrite historical truth. Multi-plant benchmarking for partner networks may be hosted at this tier under appropriate data-sharing agreements.

**10.4 Security and OT/IT Discipline**

Security in WaterOS is not limited to identity and access control. It includes evidence security: every event must be attributable, every transformation versioned, and every suppressed claim preserved. The recommended security model uses role-based access with cryptographic signatures for critical data sources, signed model manifests, append-only ledger semantics, and clear separation among operator, scientist, auditor, and administrator privileges. Read-only operational ingestion from the OT network is the default posture.

**10.5 Interface and Visualisation**

The interface philosophy is disciplined minimalism. WaterOS must not mimic consumer dashboards or flatten scientific nuance into colour-coded theatre. The essential views are a live process scoreboard, a runbook card, a phase timeline, a source-context panel, a suppressed-claim panel, and a replay view. Scientists and auditors need additional access to coefficient registries, uncertainty details, and calibration backlog objects. A mature deployment exposes a replay theatre in which users can scrub through a disturbance window and see the exact change in module scores, route decisions, and suppressed claims over time.


**11. Validation Plan — Epistemic Boundary**

This section is the credibility centre of REV3. The architecture is only useful if it states precisely what it can claim now and what it cannot claim until the domain authority and plant partners calibrate it against reality.

**11.1 What REV3 Can Claim**

REV3 can claim that a provenance-first, phase-aware governance architecture is structurally required by the literature. It can claim that the dual-window model is appropriate because the published evidence clearly shows meaningful time structure both within filter cycles and across post-fire hydrologic conditions. It can claim that arithmetic average log-reduction (μ\_LR) is inadequate as a sole adequacy descriptor and that the effective log-reduction formula μ\_LR − ½·ln(10)·σ²\_LR is the required alternative (Schmidt et al., 2020). It can claim that treatment-route logic should be auditable, that non-detect and low-count interpretations require uncertainty and recovery context, and that deterministic non-claim rules improve scientific honesty.

**11.2 What Requires Joint Empirical Calibration**

REV3 cannot claim universal thresholds for PJI (theta\_1, theta\_2), RIS state boundaries, CAS weights, CSI adequacy levels, or ELS classification boundaries. It cannot claim universal zeta-potential decision bands, UV254 triggers, dose deltas, or filter-age cutoffs. It cannot claim that the 8–12× coagulant demand escalation ratio applies uniformly to all utility-source-water combinations without site-specific verification. It cannot claim that the illustrative reserve thresholds of 12, 24, and 48 hours are correct for every utility. These matters require joint calibration with the methodology authorities (University of Waterloo wildfire-treatability research lineage) and participating utilities. The empirical data contract governing calibration readiness, schema requirements, harmonisation, and acceptance criteria is defined in Appendix E.

**11.3 Planned Timeline and Named Calibration Parameters**

| **Calibration stage** | **Duration** | **Named parameters** | **Lead reviewers** |
| - | - | - | - |
| Stage A — Historical replay | 6–10 weeks | theta\_1, theta\_2, baseline envelopes, threshold-exceedance burden weights (a1–a7) | Domain authority + plant data team |
| Stage B — Shadow mode pilot | 8–12 weeks | Route confidence weights (w1–w5, v1–v5), reserve compression markers, uncertainty penalties, zeta-potential band calibration | Domain authority + operator leads |
| Stage C — Assisted live mode | 8–16 weeks | Intervention-fit coefficients (e1–e5), alert severity cutpoints, certification evidence requirements | Domain authority + utility governance group |
| Stage D — REV4 calibration sign-off | 4–6 weeks | Approved coefficient registry and prohibited-claim refinements | Named authority + implementation sponsor |

**11.4 Test Vectors**

| **TV** | **Scenario** | **Expected WaterOS behaviour** |
| - | - | - |
| TV-01 | Post-fire raw-water turbidity and UV254 rise during hydrograph recession while reserve remains adequate | Growth rises, route optimisation suggested, no breach claim emitted. |
| TV-02 | End-of-run particle and turbidity jitter while average performance history still looks acceptable | PJI flags Early Drift (above theta\_1); strong adequacy claim suppressed. |
| TV-03 | Non-detect pathogen-related sample with recovery below 75% confidence and no E10 Recovery Update ingested | Safety claim blocked by PC-05 and PC-06; uncertainty escalation required. |
| TV-04 | Insufficient turbidity removal corrected by higher coagulant dose, but DBP pressure rises | Command module surfaces tradeoff; single-objective win claim blocked. |
| TV-05 | Alternate source available during acute post-fire transport event | Reserve score improves; route may prioritise source switching to buy optimisation time. |
| TV-06 | Catastrophic debris-flow type event beyond preparedness envelope | System escalates beyond normal route logic and refuses false mitigation claims; BAER boundary noted. |
| TV-07 | Certification attempt using arithmetic average μ\_LR = 3.875 (23h at 4-log, 1h at 1-log) | μ\_LR adequacy claim blocked by PC-01; effective log-reduction (~2.37) required; approximately 32× passage gap surfaced in ledger. |

*All threshold-bearing test vectors remain partially synthetic in REV3.1. Their purpose is architectural validation, not proof of final local calibration.*


**12. Historical Counterfactual Validation — Lost Creek Wildfire (2003)**

This section demonstrates that WaterOS is not a theoretical architecture. It applies the governance layer, gate by gate, to a real wildfire event documented in the published literature and known to the domain authority from her own research. The structure follows the pre-mortem method: historical event chronology first, then the WaterOS-governed counterfactual showing where the architecture would have intervened before the crisis became visible.

**12.1 Why This Event Was Selected**

The Lost Creek wildfire of 2003 in the Oldman River headwaters region of Alberta is the direct evidentiary foundation of Emelko et al. (2011) — the paper that established the quantitative anchors for WaterOS. That paper monitored four catchments for four years following the fire, producing the treatment-challenge data that appears throughout this specification: the 95th-percentile turbidity and DOC exceedances, the design-threshold exceedance rates, the coagulant-demand jar-test evidence, and the argument for source water supply and protection as a governing concept. The Lost Creek event is therefore the most epistemically honest choice for a WaterOS counterfactual: every number used in the walkthrough can be traced to the same paper that grounds the architecture itself.

This selection is also an explicit strategic choice. The purpose of this section is not to demonstrate the architecture against an event that is convenient for the architect. It is to demonstrate it against an event that the domain authority studied personally. Emelko et al. (2011) is her published work. When she reads the WaterOS governance walkthrough against Lost Creek, she is not being asked to imagine a scenario. She is seeing her own empirical case converted into an auditable process.

*Companion case in development: The Okanagan Mountain Park Fire (Kelowna, BC, 2003) is a stronger candidate for public institutional recognition and will be developed as a companion case for REV4. It has not been included here because the utility-level chronology — raw-water timeline, treatment-plant operating posture, operator visibility, and specific threshold crossings — requires a clean source chain that is not yet in hand. Including it without that chain would violate the same provenance-first invariant WaterOS enforces.*

**12.2 Event Background**

The Lost Creek wildfire burned in the headwaters of the Oldman River basin in southwest Alberta during the summer of 2003. The fire affected forest cover and organic soils in multiple catchments that supply surface water to downstream communities. Post-fire monitoring was conducted across burned, salvage-logged, and reference catchments, tracking the watershed and source-water implications of the disturbance over a four-year period. The Emelko et al. (2011) study represents one of the most rigorously documented post-fire treatability datasets in the Canadian literature.

The study found that source-water challenge from burned and salvage-logged catchments differed materially from reference conditions not just in average level but in the structure of variability. The 95th-percentile turbidity values were 15.3 NTU for burned and 18.8 NTU for salvage-logged catchments, compared to 5.1 NTU for reference. The 95th-percentile DOC values were 4.6 mg/L and 9.9 mg/L against 3.8 mg/L for reference. Design-threshold exceedance rates at \>10 NTU rose from approximately 2% in reference catchments to 11% in burned and 16% in salvage-logged catchments. For DOC exceeding 4 mg/L, the reference rate was approximately 4%; burned catchments approximately 9%; salvage-logged catchments approximately 48%. These numbers describe a sustained structural shift in water quality distribution rather than a temporary spike (Emelko et al., 2011).

On the treatment side, jar tests using catchment water showed coagulant demand scaling by a factor of 8–12 times under impacted conditions: a plant managing at approximately 5 mg/L PACl under reference source-water conditions required 40–60 mg/L PACl to achieve comparable settling performance under burned or salvage-logged conditions (Emelko et al., 2011). That shift does not register on a turbidity compliance chart. It registers on chemical supply, reserve integrity, and operational flexibility.

**12.3 Observed Failure Trajectory**

The following chronology describes the type of failure pattern documented in the post-fire treatability literature for affected surface-water treatment plants. This reconstruction is drawn from the documented evidence in Emelko et al. (2011) and the broader wildfire-treatability literature. It does not claim to represent a specific plant operator’s exact internal log. It reconstructs the governance failure that the published evidence makes visible.



| **Phase** | **Conditions** | **What was visible** | **What was not visible** | **Governance gap** |
| - | - | - | - | - |
| Fire season — disturbance | Wildfire burning in source watershed; organic soils exposed; ash deposition potential. | Smoke visible. Watershed staff aware of fire perimeter. | Treatment implications of source-water shift: future turbidity distribution, DOC load, coagulant demand profile. | No disturbance declaration reached treatment plant. Fire treated as land management event, not treatability event. |
| Weeks 1–4 post-fire — quiet recession | Watershed hydrology returning to base conditions. No major rainfall. | Turbidity and DOC at or near reference levels. Jar tests still in normal range. | Burned organic soils and ash now mobilisation-ready for first significant rain. Structural change in watershed is already locked in. | Treatment monitoring frequency unchanged. No elevated baseline established. Window context absent. |
| First major stormflow event | Rainfall drives runoff over burned slopes. Turbidity and DOC spike sharply and non-linearly. | Turbidity rises. Operators see value exceed reference levels. | How high the 95th percentile will go. Whether the coagulant dose still achieves required settling. Whether the filter is entering end-of-run or early breakthrough while turbidity is still nominally acceptable. | Average turbidity used as control signal. End-of-run filter phase undetected. Coagulant response lags the actual demand by hours. |
| Treatment stress accumulation | Coagulant demand has increased 8–12× beyond reference requirements. Settled-water quality marginal. Filter runs shortened by solids load. | Coagulant dose is higher than normal. Operators are actively adjusting. | Reserve integrity: how many more escalation steps remain before design capacity is exceeded. Effective log-reduction vs. arithmetic average during this cycle. Whether the system is already leaking while turbidity reads acceptable. | No reserve tracking. Effective LR not computed. No phase-aware governance. Arithmetic average used for compliance narrative. |
| Latency and delayed recognition | Treatability degradation has been accumulating for days. Effective protection has weakened even while daily compliance reports appear acceptable. | End-of-cycle turbidity averages may still be within compliance range. | That the process is operating in early or late breakthrough. That effective LR may have dropped from ~5-log (stable) toward 2–1 log. That the 32× passage gap dynamic may already be in play. | No mechanism to surface the discrepancy between arithmetic average and effective performance. Governance gap persists until visible exceedance forces a reactive response. |

**12.4 WaterOS Gate-by-Gate Counterfactual**

The following table describes how WaterOS governance would have progressed through the same event, using the event grammar, module logic, and gate structure defined in Sections 4 through 9. Sensor envelope assumed: continuous raw-water turbidity and UV254, filter-effluent turbidity, coagulant dose history, filter runtime, historical baseline window from reference and pre-fire records.

| **Phase** | **WaterOS event / gate** | **Module response** | **Governance outcome vs. actual** |
| - | - | - | - |
| Fire ignition in source watershed | E1 Disturbance Declaration ingested from wildfire service perimeter update. Foundation module sets watershed state to "Disturbed." CSI updated to reflect reduced baseline confidence. | Foundation module: baseline window shifts from "Anchored" to "Disturbed context." Monitoring cadence elevated. Growth module begins tracking for hydrologic stress flag. | Actual: no treatment-governance action taken. WaterOS: plant is already on elevated watch two to four weeks before any water quality change is visible. |
| Quiet recession period — weeks 1–4 | No E2 or E3 events triggered; no sustained threshold exceedance. System runs normally in Disturbed context. V3 Context Sufficiency gate holds CSI in degraded-context mode. | No false alarm. System does not escalate without evidence. Reserve module begins tracking chemical stock relative to the known escalation risk from the Disturbance Declaration. | Actual: normal operations, no preparation. WaterOS: reserve watch is active; chemical buffer is being monitored against the 8–12× demand scenario documented in the literature for this disturbance type. |
| First stormflow onset | E2 Hydrologic Regime Shift triggered: stormflow onset. E3 Raw-Water Excursion triggered: ROC\_turbidity and ROC\_uv254 both rising. Growth Pressure Index moves to "Rising." | V4 Process Physics gate computes GPI and begins updating PJI. Reserve Integrity Score begins descending from Buffered toward Narrowing. Command module evaluates whether current coagulant dose matches new challenge class. | Actual: coagulant dose begins to increase; no phase tracking; no reserve or route coherence check. WaterOS: coherence deficit between dose and challenge logged; route optimisation path opened. |
| Filter cycle enters end-of-run under elevated load | E5 Filter Phase Transition triggered: PJI exceeds theta\_1 (Early Drift). Filter age pressure elevated. System computes effective LR using μ\_LR and σ²\_LR from available cycle data. | V6 Prohibited Content gate: PC-01 blocks any adequacy claim using arithmetic average alone. PC-02 blocks any pathogen security claim from turbidity compliance alone. ELS rises; Entropy module moves to "Seeping." | Actual: operator sees turbidity values that are still below regulatory thresholds; no governance action taken. WaterOS: adequacy claim suppressed. Operator notified of end-of-run phase onset while turbidity is still nominally acceptable. |
| Coagulant demand approaching design capacity | E6 Reserve Compression triggered: coagulant flexibility index drops. RIS transitions from Narrowing to Compressed. Reserve time estimate falls below 24 hours at current demand rate. | V5 Route Logic gate selects alkalinity support and dose-range review path from the tradeoff matrix. Socratic Handshake Gate requires operator acknowledgement that chemical restocking has been arranged or that source switching is being evaluated. | Actual: plant continues on reactive manual adjustment. Chemical reserve may not be flagged until it is nearly exhausted. WaterOS: reserve compression alert issued at Narrowing state, before the plant reaches Compressed — creating decision time rather than eliminating it. |
| Post-fire stormflow peak with high turbidity and DOC | E7 Entropy Leak triggered: ELS in Leaking state. Effective LR computed from cycle data shows significant gap versus arithmetic mean. PC-01, PC-02, PC-05 potentially active. V6 suppresses all strong adequacy and safety claims. | Agency Gate Checklist: AG-1 through AG-8 evaluated. If effective LR has fallen to ~3-log or below and no E10 Recovery Update is on file, AG-5 fails. Strong claims blocked. System escalates to Governance Escalation (E8). | Actual: treatment continues; boil-water advisory decision may be delayed by lack of an integrated risk signal. WaterOS: the system is in E8 Governance Escalation state. An advisory is not automatically issued by the software — but the software refuses to certify safe water and places the accountability decision explicitly with the operator. |
| Sustained high-challenge period — weeks 5–10 of post-fire recession | E2 Hydrologic Regime: system updates watershed window to post-fire recession, tracking the 3–7 week characteristic window established in the literature. Foundation module maintains Disturbed context. | System does not declare crisis over when a single storm passes. The post-fire turbidity and DOC distribution documented by Emelko et al. (2011) may persist for months and years; WaterOS keeps the elevated governance posture until the baseline window re-establishes reference characteristics. | Actual: plants may reduce vigilance between storm events once turbidity returns to near-normal levels. WaterOS: the 95th-percentile shift documented in the literature is already encoded in the Foundation module as the reason why inter-storm conditions are not equivalent to pre-fire conditions. |

**12.5 Earliest Intervention Point**

In the WaterOS-governed counterfactual, the governance posture changes at the moment of the Disturbance Declaration — which is the point at which a wildfire perimeter update reaches the Foundation module. That is not when the water changes. That is when the meaning of all future water quality signals changes, because the context in which they will be interpreted has already shifted. A turbidity reading of 3 NTU in a reference catchment context and a turbidity reading of 3 NTU in a disturbed-watershed context are not the same governance signal. WaterOS encodes that difference as a first-class distinction.

The first concrete governance action changes between the actual and counterfactual timelines at the onset of the first stormflow event. At that point, the actual plant sees turbidity rising and adjusts coagulant dose reactively, without phase or reserve context. The WaterOS-governed plant receives an E3 Raw-Water Excursion and E2 Hydrologic Regime Shift simultaneously; the Growth module, Reserve module, and Command module all update; PJI begins rising; and the coherence check between the dose and the challenge class begins immediately. The adequacy claim suppression — PC-01 and PC-02 blocking any turbidity-based safe-water narrative — is in place before the effluent turbidity has moved enough to trigger a conventional alarm.

**12.6 Counterfactual Outcome**

The following outcomes are claimed on the basis of the architectural governance differences described above. They are stated as governance improvements, not as guaranteed physical outcomes, because the physical outcomes depend on local calibration, operator response, and infrastructure capacity that this pre-mortem cannot specify.

| **Governance improvement** | **Basis** | **Epistemic confidence** |
| - | - | - |
| Treatment-challenge shift recognised at disturbance declaration, not at regulatory exceedance | Foundation module responds to E1 event from wildfire service. No analogous trigger in conventional monitoring. | High — this is a structural governance difference independent of calibration. |
| End-of-run filter phase identified before turbidity compliance breach | PJI logic and E5 Filter Phase Transition events; consistent with Emelko et al. 2003 finding that deterioration precedes turbidity breach. | High given continuous sensor data; moderate given PJI threshold calibration dependency. |
| Adequacy and safety claims suppressed during elevated uncertainty window | PC-01 through PC-06 mechanically block claims that cannot be truthfully supported given the effective LR gap and the absence of an E10 Recovery Update. | High — this is a hardcoded architectural rule, not a calibration-dependent outcome. |
| Reserve compression detected at Narrowing state rather than at Exhausted state | RIS tracking translates the 8–12× coagulant demand escalation pattern into a forward-looking reserve signal. Creates decision time before capacity is exceeded. | Moderate — depends on RIS coefficient calibration and chemical inventory data quality. |
| Post-storm governance posture maintained across full post-fire recession window | Foundation module keeps the Disturbed watershed context active for the duration of the 3–7 week recession window; does not declare return to normal between individual storm events. | High — structural feature of the dual-window model. |

**12.7 Epistemic Boundary of This Counterfactual**

This section makes claims about governance structure, not about prevented harm. The difference is precise. What can be claimed: WaterOS would have changed what was visible, when it was visible, and what could be claimed on the basis of it. What cannot be claimed without plant-specific data: the exact number of hours earlier an advisory would have been issued, the exact reduction in chemical cost, or the exact pathogen passage averted. Those outcomes depend on local calibration, operator decision quality, infrastructure capacity, and factors outside the system’s governance.

The reason for this epistemic boundary is not weakness. It is the same discipline that makes Section 11 credible. WaterOS is a governance architecture, not a promise. The proof-of-governance is that the architecture changed what the operator could see and what the system would allow to be claimed. In a domain where the central failure is the latency between correctable shifts and institutional recognition of those shifts, earlier visibility and suppressed false assurance are themselves the measurable outcome.

This counterfactual will be updated in REV4 with site-specific calibrated thresholds once Stage A–B data from a partner utility is available. The current pre-mortem is a structural demonstration. The calibrated pre-mortem will be a quantitative one.

*Note for domain authority: the Lost Creek / Oldman River Basin case was selected precisely because it is already in your published lineage. If the event chronology presented here is incomplete or imprecise in any way, we request corrections during REV3.1 review. Section 12 is intended to be co-authored with the domain authority, not presented as an independent reconstruction of events she knows better than anyone.*


**Appendix A: Event Model and Canonical Schemas**

Appendix A expands the event model into canonical objects for implementation teams. Each schema is normative for REV3, subject to field extension but not field repurposing.

**A.1 Disturbance Declaration Schema**

\{  
  "event\_type": "E1\_DISTURBANCE\_DECLARATION",  
  "disturbance\_kind": "wildfire|ashfall|extreme\_rain\_over\_burn|debris\_flow\_warning",  
  "spatial\_reference": "watershed / intake / basin id",  
  "severity\_class": "watch|warning|acute",  
  "supporting\_source": "agency notice or verified field source",  
  "confidence\_class": "reported|validated|uncertain"  
\}

**A.2 Filter Phase Transition Schema**

\{  
  "event\_type": "E5\_FILTER\_PHASE\_TRANSITION",  
  "filter\_id": "string",  
  "phase\_from": "ripening|stable|end\_of\_run|early\_breakthrough|late\_breakthrough",  
  "phase\_to": "ripening|stable|end\_of\_run|early\_breakthrough|late\_breakthrough",  
  "runtime\_minutes": 0,  
  "supporting\_evidence": \{  
    "effluent\_turbidity\_ntu": 0.0,  
    "particle\_count\_per\_ml": 0,  
    "phase\_jitter\_index": 0.0  
  \}  
\}

**A.3 Recovery Update Schema**

\{  
  "event\_type": "E10\_RECOVERY\_UPDATE",  
  "organism": "Cryptosporidium|Giardia|surrogate",  
  "method": "USEPA\_1623|equivalent",  
  "recovery\_mean": 0.75,  
  "recovery\_std": 0.16,  
  "distribution": "beta",  
  "alpha": 287.08,  
  "beta": 94.76,  
  "effective\_date": "ISO8601 timestamp",  
  "lab\_reference": "LIMS record identifier"  
\}

**A.4 Governance Escalation Schema**

\{  
  "event\_type": "E8\_GOVERNANCE\_ESCALATION",  
  "reason": "missing\_provenance|uncertainty\_overflow|prohibited\_claim|reserve\_exhaustion",  
  "required\_actor": "operator|scientist|administrator",  
  "claim\_suppressed": true,  
  "suppressed\_claim\_type": "adequacy|safety|optimisation|certification",  
  "prohibition\_code": "PC-01|PC-02|PC-03|PC-04|PC-05|PC-06",  
  "comment": "human-readable explanation"  
\}


**Appendix B: Canonical Mathematics**

Appendix B contains all canonical formulas and the variable dictionary that governs their interpretation. Formulas in B.1 through B.6 are binding only in conjunction with the variable definitions in B.0. No implementation may rename, repurpose, or silently reinterpret a formula term. Coefficients not tied to published source values are explicitly labelled pending calibration.

**B.0 Canonical Variable Dictionary**

The dictionary below defines every synthetic metric and every sub-variable that appears in the formulas in B.1 through B.6. Each entry specifies the variable's role, unit, expected range, directionality, time basis, source fields, missing-data rule, and calibration status. An implementation that does not adhere to these definitions is not a WaterOS implementation.

| **Variable** | **Plain meaning** | **Unit** | **Range** | **Direction** | **Time basis** | **Missing-data rule** | **Calib. status** |
| - | - | - | - | - | - | - | - |
| μ\_LR (mu\_LR) | Arithmetic mean of per-cycle log-removal values | log₁₀ units | 0–6+ | Higher = better removal | Computed over completed filter cycle | Require minimum 3 measurements; if fewer, emit Governance Escalation E8 | Fixed (from literature) |
| σ²\_LR (sigma-sq\_LR) | Variance of per-cycle log-removal values | log₁₀ units squared | 0–∞ | Higher = more variable (worse) | Same cycle as μ\_LR | If only 1 measurement, variance is unknown; block effective LR calculation | Fixed (from literature) |
| LR\_effective     | Effective log-reduction accounting for variability | log₁₀ units | 0–6+ | Higher = better | Cycle-level output | Cannot compute without σ²\_LR; block risk claims | Fixed (B.1 formula) |
| GPI (Growth Pressure Index) | Composite measure of raw-water challenge acceleration | Normalised \[0–1+\] | 0–1+ | Higher = greater challenge | Rolling 15–60 min window | If turbidity absent, use worst-case 95th percentile; degrade to Tier C | Pending calibration |
| RIS (Reserve Integrity Score) | Composite measure of remaining treatment manoeuvre room | Normalised \[0–1\] | 0–1 | Higher = more reserve | Current operational state | If inventory data absent, assume Compressed; log data gap | Pending calibration |
| CAS (Coherence Alignment Score) | Measure of how well treatment actions match current challenge | Normalised \[0–1\] | 0–1 | Higher = better aligned | Updated on each Treatment Adjustment event (E4) | If route history absent, default to 0.5; flag route uncertainty | Pending calibration |
| CSI (Context Sufficiency Index) | Measure of whether enough baseline and process context exists | Normalised \[0–1\] | 0–1 | Higher = richer context | Assessed at each V3 gate pass | CSI = 0 if no baseline window; block all strong claims | Pending calibration |
| ELS (Entropy Leak Score) | Composite measure of process integrity loss across all leak modes | Normalised \[0–1+\] | 0–1+ | Higher = more leakage | Rolling filter-cycle window | If effective LR unavailable, ELS cannot be computed; block passage risk claims | Pending calibration |
| PJI (Phase Jitter Index) | Composite measure of cross-sensor discordance signalling early drift | Normalised \[0–1+\] | 0–1+ | Higher = more jitter | Rolling 30-min to 2-hour window | If fewer than 2 signals available, PJI is undefined; treat as theta\_1 alert | Pending calibration |
| TM (Treatability Margin) | Net difference between capacity and disturbance load | Composite index | −∞ to +∞ | Positive = functional; negative = failing | Updated each physics cycle | If any primary module score unavailable, TM is undefined; issue S2 posture | Pending calibration |
| ROC\_turbidity | Rate of change of raw-water turbidity | NTU/h | −∞ to +∞ | Rising positive = challenge growth | Computed over rolling 15-min window | If turbidity missing \>15 min, set to worst-case historical rate; log gap | Pending calibration |
| ROC\_uv254 | Rate of change of UV254 absorbance | 1/cm per hour | −∞ to +∞ | Rising positive = organic load growing | Rolling 15-min window | If UV254 unavailable, set ROC\_uv254 = 0; reduce CAS confidence | Pending calibration |
| FilterAgePressure | Normalised elapsed runtime relative to historical end-of-run timing | Normalised \[0–1\] | 0–1 | Higher = closer to historical end-of-run | Updated continuously; resets at backwash | If cycle start time unknown, estimate from historical pattern; apply uncertainty penalty | Pending calibration |
| zeta\_potential\_deviation | Signed distance of observed zeta potential from the −3 to +1 mV operating window | mV | −∞ to +∞ | Zero = optimal; larger deviation = worse | Instantaneous; updated on each measurement event | If zeta unavailable, set deviation = unknown; reduce CAS; degrade to Tier B | Pending calibration |
| RecoveryUncertaintyPenalty | Penalty applied when recovery distribution is absent or stale (no E10 event within 90 days) | Normalised \[0–1\] | 0–1 | Higher = more uncertainty burden | Triggered by absence of current E10 event | Default to maximum penalty (1.0) if no E10 on file; block PC-06 claims | Pending calibration |

*Variables labelled "Pending calibration" have architecturally justified formula roles but coefficient values that must be determined through the Stage A–D programme in Appendix E. Variables labelled "Fixed (from literature)" or "Fixed (B.1 formula)" have form specified by published mathematics and are not subject to fitting.*

**B.1 Effective Log-Reduction Formula**

This formula is architecturally required, not estimated. No WaterOS output may claim adequacy using μ\_LR alone.

\# Effective log-reduction (Schmidt, Anderson & Emelko, 2020)  
  
LR\_effective = μ\_LR - (0.5 × ln(10) × σ²\_LR)  
  
\# where:  
\#   μ\_LR   = arithmetic mean of per-cycle log-reduction values  
\#   σ²\_LR  = variance of per-cycle log-reduction values  
\#   ln(10)  = 2.302585...  
  
\# Worked example (from Section 5.5 and TV-07):  
\#   Process at 4-log for 23 h, 1-log for 1 h  
\#   μ\_LR  = (4×23 + 1×1) / 24 = 3.875  
\#   LR\_effective ≈ 2.37  (approximate; σ²\_LR required for exact calculation)  
\#   Passage gap: 10^(3.875 - 2.37) ≈ 32× more passage than μ\_LR implies

**B.2 Growth Pressure Index**

GPI = c1×ROC\_turbidity + c2×ROC\_uv254 + c3×ROC\_particle\_count  
    + c4×DOC\_displacement + c5×hydrologic\_stress\_flag  
  
\# All coefficients c1–c5 pending calibration (see Appendix E, Stage A).

**B.3 Reserve Integrity Score**

RIS = d1×alkalinity\_margin + d2×coagulant\_flexibility  
    + d3×backup\_source\_readiness + d4×filter\_productivity\_margin  
    - d5×solids\_burden  
  
\# State thresholds: Buffered (\>48h), Narrowing, Compressed, Exhausted (\<12h)  
\# All d-coefficients and time thresholds pending calibration.

**B.4 Coherence Alignment Score**

CAS = e1×intervention\_fit + e2×route\_consistency  
    + e3×operator\_acknowledgement - e4×override\_conflict  
    - e5×zeta\_potential\_deviation  
  
\# Target zeta window: -3 mV to +1 mV (Gifford, 2025)  
\# All e-coefficients pending calibration.

**B.5 Entropy Leak Score**

ELS = f1×breakthrough\_signal + f2×effective\_passage\_risk  
    + f3×dbp\_pressure + f4×taste\_odour\_burden  
  
\# effective\_passage\_risk MUST use LR\_effective, not μ\_LR  
\# All f-coefficients pending calibration.

**B.6 Phase Jitter Index**

PJI = a1×|d(raw\_turbidity)/dt|\_norm + a2×|d(particle\_count)/dt|\_norm  
    + a3×|d(uv254)/dt|\_norm + a4×FilterAgePressure  
    + a5×ResponseMismatchPenalty + a6×RecoveryUncertaintyPenalty  
    - a7×ContextSufficiencyIndex  
  
\# Placeholder thresholds (pending calibration):  
\#   theta\_1 (Early Drift): normalised turbidity slope \>0.1 h⁻¹  
\#              OR turbidity trend \>0.005 NTU/h over 2-hour window  
\#   theta\_2 (Terminal Approach): turbidity 0.1–0.3 NTU with positive  
\#              acceleration, OR PJI composite \>0.3  
  
\# All a-coefficients pending calibration.

*Pending calibration means exactly that: the formula family is architecturally justified, but coefficient values require plant-specific fitting and scientist review under the Stage A–D programme in Appendix E. The LR\_effective formula in B.1 is not pending calibration — its form is fixed by the published mathematics.*


**Appendix C: Validator Node JSON Contracts**

These contracts define what each validator stage must receive and produce. They are intended for implementation planning and interface discipline.

**C.1 V3 Context Sufficiency Contract**

\{  
  "input": \{  
    "event\_ref": "string",  
    "baseline\_window\_ref": "string|null",  
    "disturbance\_context": "object|null",  
    "filter\_phase\_context": "object|null",  
    "recovery\_distribution": "object|null"  
  \},  
  "output": \{  
    "context\_sufficiency\_index": 0.0,  
    "missing\_elements": \["baseline\_window\_ref"\],  
    "status": "pass|degraded|fail"  
  \}  
\}

**C.2 V6 Prohibited Content Contract**

\{  
  "input": \{  
    "candidate\_claim": "string",  
    "supporting\_state": "object",  
    "uncertainty\_context": "object",  
    "log\_reduction\_method": "average|effective",  
    "recovery\_event\_on\_file": true,  
    "recovery\_credible\_interval\_upper": 0.0  
  \},  
  "output": \{  
    "allowed": false,  
    "violation\_type": "PC-01|PC-02|PC-03|PC-04|PC-05|PC-06",  
    "required\_substitution": "insufficient\_basis\_or\_escalate"  
  \}  
\}


**Appendix D: Validation Matrix**

The matrix below expands test vectors into reviewable expectations. No code path is considered complete until it maps to at least one approved validation vector and one prohibited non-claim.

| **TV** | **Input pattern** | **Gate path** | **Expected output** | **Non-claim requirement** |
| - | - | - | - | - |
| TV-01 | Rising raw turbidity + UV254 in post-fire recession | V0→V5 pass; V6 clear | Optimisation path with reserve watch | No claim that challenge is catastrophic without corroboration |
| TV-02 | Stable averages but rising PJI near end-of-run | V0→V4 pass; V6 blocks strong adequacy | Early Drift warning (theta\_1 exceeded) | Do not claim adequacy from μ\_LR history alone |
| TV-03 | Non-detect with no E10 Recovery Update on file (recovery below 75%) | V0→V3 degraded; V6 PC-05 and PC-06 hit | Uncertainty escalation; safety claim blocked | Do not claim zero risk |
| TV-04 | Improved turbidity removal with worsened DBP pressure | V0→V5 pass | Tradeoff alert; dual-objective route surfaced | Do not present single-metric victory |
| TV-05 | Alternate source available under acute event | V0→V5 pass | Source-switch recommendation optional | Do not imply universal superiority of switching |
| TV-06 | Catastrophic debris-flow type condition | V0→V4 fail-safe escalation | Emergency mode; BAER boundary noted; limited claims | Do not claim routine preparedness can reasonably mitigate |
| TV-07 | Certification attempt using μ\_LR = 3.875 (23h at 4-log, 1h at 1-log) | V6 PC-01 triggered | Effective LR (~2.37) required; ~32× passage gap logged | Do not certify on μ\_LR alone |


**Appendix E: Calibration Dataset Specification**

This appendix defines the empirical data programme required to advance WaterOS from REV3 architecture to REV4 calibrated deployment. Every uncalibrated coefficient named in Appendix B is represented here with a named calibration method, required dataset characteristics, and sample size reasoning. No coefficient may be claimed as fitted until this programme is completed and accepted by the domain authority.

**E.1 Parameters Requiring Calibration**

| **Parameter** | **Hosting formula** | **Current status** | **Calibration method** | **Dataset requirement** |
| - | - | - | - | - |
| theta\_1 (PJI Early Drift threshold) | Appendix B.6 | Engineering placeholder | Historical replay; maximise precision-recall for early drift detection against phase-labelled records | Min. 50 complete filter cycles from ≥2 utilities with phase-labelled records |
| theta\_2 (PJI Terminal Approach threshold) | Appendix B.6 | Engineering placeholder | Fitted jointly with theta\_1; secondary threshold | Same dataset as theta\_1 |
| c1–c5 (GPI coefficients) | Appendix B.2 | Engineering estimate | Regression against post-fire turbidity/DOC time-series; cross-validation with Emelko et al. (2011) exceedance benchmarks | Min. 2 post-fire seasons of continuous raw-water data |
| d1–d5 (RIS coefficients) and reserve time thresholds | Appendix B.3 | Engineering estimate | Operational review with utility operators; reserve depletion events as calibration anchors | Min. 5 documented stress events per utility; chemical consumption logs |
| e1–e5 (CAS coefficients incl. zeta-potential weight) | Appendix B.4 | Engineering estimate | WRF 5168 tradeoff matrix cross-validation; zeta-potential operating envelope from Gifford (2025) | Min. 30 coagulation optimisation records with concurrent zeta and performance data |
| f1–f4 (ELS coefficients) | Appendix B.5 | Engineering estimate | Paired filter-cycle and effective LR dataset; entropy states labelled against Emelko et al. (2003) phase definitions | Min. 30 filter cycles with lab-confirmed pathogen or surrogate passage data |
| a1–a7 (PJI coefficients) | Appendix B.6 | Engineering estimate | Optimisation against early-warning lead time on phase-labelled dataset | Same dataset as theta\_1/theta\_2 |
| w1–w5, v1–v5 (TM capacity/load weights) | Section 6.1 | Engineering estimate | Sensitivity analysis and cross-utility validation after primary module coefficients are calibrated | Requires at least Stage B completion first |

**E.2 Sample Size Reasoning**

The minimum sample sizes above derive from two considerations. Statistically, fitting a five-term index requires at minimum 50–100 observations to avoid overfitting, and 30 represents the credible lower bound for parameter estimation in a domain-specific context. Domain-specifically, wildfire seasons are episodic and post-fire data is inherently limited; REV4 calibration must therefore draw on historical archives from multiple utilities and multiple post-fire seasons.

**E.3 Statistical Methods**

Stage A (historical replay) will use penalised regression with cross-validation to fit index coefficients, with holdout validation against independently collected utility records. Stage B (shadow mode) will use paired comparison of WaterOS predictions against expert operator judgements to validate route selection logic. Stage C (assisted live mode) will use Bayesian updating to refine coefficients and threshold estimates as new data accumulates. The LR\_effective formula (Appendix B.1) is not subject to fitting: its form is fixed by the published mathematics and requires only that per-cycle μ\_LR and σ²\_LR be measurable.

**E.4 Analysis Pipeline**

Phase 1: Data assembly and cleaning (weeks 1–4 of Stage A). Phase 2: Baseline envelope construction and exceedance labelling (weeks 5–10). Phase 3: Coefficient fitting with cross-validation and uncertainty quantification (weeks 11–16). Phase 4: Threshold calibration and test-vector validation against Appendix D matrix (weeks 17–20).

**E.5 Calibration Deliverables and Signature Block**

| **Deliverable** | **Stage** | **Format** | **Recipient** |
| - | - | - | - |
| Coefficient registry draft (c1–f4, a1–a7) | Stage A | Signed JSON registry + statistical report | Domain authority for review |
| Threshold calibration report (theta\_1, theta\_2, reserve thresholds) | Stage B | Technical memo with confidence intervals | Domain authority + utility operators |
| Test-vector validation against Appendix D | Stage C | Appendix D matrix with pass/fail annotations | Domain authority |
| REV4 calibration sign-off | Stage D | Signed coefficient registry + prohibited-claim refinements | Domain authority + author |


Calibration programme sponsor: Regis Benoit Brice Nde Tene, Sovereign Process Architecture

Methodology reference authority: University of Waterloo (Emelko research lineage)

Programme status: Pending initiation — subject to institutional engagement and data access agreement


**References**

Emelko, M.B., Silins, U., Bladon, K.D., & Stone, M. (2011). Implications of land disturbance on drinking water treatability in a changing climate: Demonstrating the need for source water supply and protection strategies. Water Research, 45, 461–472.

Schmidt, P.J., Anderson, W.B., & Emelko, M.B. (2020). Describing water treatment process performance: Why average log-reduction can be a misleading statistic. Water Research, 176, 115702.

Emelko, M.B., Huck, P.M., & Douglas, I.P. (2003). Cryptosporidium and microsphere removal during late in-cycle filtration. Journal AWWA, 95(5), 173–180.

Emelko, M.B., Schmidt, P.J., & Reilly, P.M. (2010). Particle and microorganism enumeration data: Enabling quantitative rigor and judicious interpretation. Environmental Science & Technology, 44(5), 1720–1727.

Gifford, M. (2025, May 8). Building Treatment Resilience to Wildfires through Conventional Filtration Piloting: WRF 5168 Enhancing Drinking Water Treatment Resilience to Wildfire Events. Portland Water Bureau presentation.

Water Research Foundation / Canadian Water Network. (2014). Wildfire Impacts on Water Supplies and the Potential for Mitigation: Workshop Report (Web Report \#4529).

*Note on citation precision: pinpoint page and table citations for all numerical anchors (filter-phase log-removal values, 95th-percentile turbidity/DOC figures, coagulant demand scaling, effective LR formula location, zeta-potential slide number) are to be confirmed against the physical papers and presentation during empirical calibration with appropriate domain authorities. The reference list above identifies document, year, and claim with confidence; page-level precision requires access to the physical sources.*


*© 2026 Regis Benoit Brice Nde Tene. All rights reserved.*

*Built on publicly available University of Waterloo wildfire-treatability research (Emelko et al. 2011, 2003, 2010; Schmidt et al. 2020) and peer-reviewed water treatment literature. All methodology lineage cited in References.*

---

## Topics

**Domain:** wildfire-treatability · source-water disturbance · drinking-water treatment · water utility resilience · post-wildfire watershed · turbidity · dissolved organic carbon · DOC · coagulant demand · zeta potential · UV254 · effective log-reduction · filter-cycle dynamics · pathogen enumeration · cryptosporidium · Lost Creek wildfire · Oldman River · Okanagan · University of Waterloo · WRF · Canadian water utility · resilience piloting

**Methodology:** AI governance · deterministic AI · AI audit trail · AI lifecycle controls · AI compliance evidence · methodology specification · process governance · regulated industries AI · Sovereign Process Architecture · scientific operating system · Flight Recorder · Validator Node Pipeline · Prohibited Content Rules · Socratic Handshake · spec-driven development
