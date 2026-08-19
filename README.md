# LIS Interface Validation Case Study

**Laboratory Information Systems | HL7 v2 | Clinical Applications | Interface Validation | Laboratory Operations**

A clinical systems case study showing how laboratory domain knowledge, interface troubleshooting, and structured root-cause analysis can be used to protect clinical meaning across the **EHR → interface → LIS → laboratory → EHR** workflow.

## Full Case Study

**[View the Laboratory Interface Safety & TAT Diagnostic Framework (PDF)](./allen_stalcup_lis_case_study.pdf)**

> **Core idea:** A successful interface transmission does not necessarily mean the clinical workflow succeeded. Patient identity, order relationships, specimen tracking, result interpretation, timestamps, and final presentation all have to remain correct from end to end.

---

## Case at a Glance

A laboratory's overall turnaround time increased by **71%**, initially pointing attention toward chemistry operations.

Instead of assuming the analytical laboratory was responsible, the investigation decomposed the workflow into individual stages and followed the evidence until the delay could be localized.

### Simulated findings

| Finding | Result |
|---|---:|
| Overall TAT | 49 → 84 min |
| Collection → lab receipt | 18 → 54 min |
| Lab receipt → final result | 31 → 30 min |
| Satellite Clinic transport | 24 → 96 min |
| Send-out staging delay | 37 → 16 min after intervention |
| Send-out SLA compliance | 72% → 96% |

The analytical laboratory remained essentially stable.

The primary deterioration occurred **upstream of testing**, eventually localizing to specimen transportation and send-out workflow controls.

---

## What This Project Demonstrates

- Laboratory Information System workflow analysis
- HL7 v2 laboratory result interpretation
- LIS/EHR interface troubleshooting
- ORU message and ACK tracing
- Patient identity integrity
- Order and accession relationship validation
- Laboratory test-code mapping
- Critical-result interpretation preservation
- Clinical timestamp validation
- Turnaround-time decomposition
- Root-cause localization
- Interface exception handling
- Negative and regression testing
- Change-control planning
- Canary validation and rollback thinking
- Laboratory operations improvement

---

## System Boundary

```text
Provider
   │
   ▼
  EHR
   │
   │ Order
   ▼
Interface Engine
   │
   ▼
  LIS
   │
   ├────► Middleware ────► Analyzer
   │                         │
   ◄─────────────────────────┘
   │
   │ Result
   ▼
Interface Engine
   │
   ▼
  EHR
   │
   ▼
Clinician
```

At each boundary, the investigation asks whether the clinical meaning remains intact:

```text
Patient identity
      ↓
Order identity
      ↓
Specimen / accession
      ↓
Test mapping
      ↓
Result value
      ↓
Units / reference range
      ↓
Abnormal / critical interpretation
      ↓
Result status
      ↓
Clinical timestamps
      ↓
Final destination
```

---

## Investigation

### 1. Validate the data before using the metric

Before analyzing turnaround time, the case first establishes that the timestamps and transactions being measured represent the expected clinical events.

A dashboard built on incorrect interface data can create a highly confident wrong conclusion.

> **Control:** Verify the integrity and meaning of the underlying transactions before using them for operational decisions.

### 2. Decompose total turnaround time

The initial assumption was that chemistry processing had become slower.

Breaking the metric into components showed otherwise:

```text
LAST MONTH

Collection → Receipt     18 min
Receipt → Final          31 min


THIS MONTH

Collection → Receipt     54 min  ← deterioration
Receipt → Final          30 min  ← stable
```

The evidence did not support changing chemistry operations.

The investigation moved upstream.

### 3. Localize the failure

Performance was then segmented by location:

```text
Emergency Department       Stable
Inpatient Floors           Stable
Main Outpatient Clinic     Stable
Satellite Clinic           24 → 96 min
```

The problem was no longer:

> "Laboratory TAT is getting worse."

It became:

> **"Pre-analytic delay is concentrated at the Satellite Clinic."**

Further decomposition showed that collection-to-pickup remained stable while **courier pickup-to-lab-receipt deteriorated sharply**.

### 4. Separate systemic deterioration from exceptions

Most specimens still traveled normally.

A subset experienced delays of roughly **150–210 minutes**.

The delayed cases were segmented by:

- location
- collection time
- courier run
- route
- specimen type
- destination
- shift/day
- pickup and receipt timestamps

The pattern eventually concentrated around specific scheduled courier runs.

> **Principle:** Patterns generate hypotheses. Transaction-level evidence assigns ownership.

### 5. Contain before redesigning

Once missed transport runs were identified as a credible failure mode, the safer sequence was:

```text
Detect risk
   ↓
Contain the failure
   ↓
Verify root cause
   ↓
Correct the process
   ↓
Validate the change
   ↓
Monitor for recurrence
```

Backup coverage and missed-pickup escalation could protect the workflow while the underlying scheduling or transport issue was investigated.

---

## A Second Failure Population

After the primary transport issue was addressed, another group of delayed specimens remained.

All were microbiology send-outs.

That changed the question from:

> "Why are these specimens outliers?"

to:

> **"Do these specimens belong to a different workflow population?"**

They did.

The send-out process was decomposed:

```text
Collection → clinic pickup          11 min
Clinic pickup → main lab            18 min
Main lab → send-out staging         37 min  ← opportunity
Staging → reference courier          8 min
```

Further inspection showed that much of the 37-minute interval was not active processing.

It was **batch waiting**.

### Operational decision

Moving every specimen immediately would minimize delay but increase staff burden unnecessarily.

Keeping the existing 30-minute batching interval would preserve efficiency but continue missing the desired service level.

The proposed compromise:

> **Reduce the staging batch interval to 10 minutes and measure both service performance and labor impact.**

### Simulated post-change result

```text
SLA compliance
72% → 96%

Main lab → staging
37 min → 16 min

Staging trips/day
12 → 30

Overtime
No meaningful increase
```

The goal was not to create the fastest theoretically possible workflow.

It was to find the **lowest-cost process that reliably met the required operating standard**.

---

# Interface Safety Controls

Operational performance is only one part of the case.

The second part examines whether the interface preserves clinical meaning.

## Example HL7 v2 Result

```hl7
MSH|^~\&|LAB_LIS|LAB|EHR|HOSPITAL|202608181451||ORU^R01|MSG12345|P|2.5
PID|1||12345^^^HOSPITAL_A^MR||DOE^JOHN||19800102
OBR|1|ORD200|ACC200|K^Potassium|||202608181400|||||||202608181423||||||||202608181451
OBX|1|NM|K^Potassium||6.8|mmol/L|3.5-5.1|HH|||F
```

Conceptually, this transaction carries:

```text
Patient:            MRN 12345
Order:              ORD200
Accession:          ACC200
Test:               Potassium
Result:             6.8 mmol/L
Interpretation:     Critical High
Status:             Final
Collection time:    14:00
Lab receipt:        14:23
Final/report time:  14:51
Message ID:         MSG12345
```

A receiving system may acknowledge the transaction:

```hl7
MSA|AA|MSG12345
```

That acknowledgment is useful evidence that the receiving application accepted the message.

It does **not**, by itself, prove that the result was:

- filed to the correct patient
- associated with the correct order
- interpreted correctly
- displayed correctly
- routed through every required clinical workflow

End-to-end validation continues beyond transmission.

---

## Silent Mapping Failure

Some of the most important interface failures do not produce an obvious error.

Consider:

```text
Provider orders:
CBC with Differential

EHR code:
CBCDIFF

Incorrect interface mapping:
CBCDIFF → CBC

LIS receives:
CBC
```

The message may transmit successfully.

The LIS may accept it.

Testing may occur.

Results may return.

Yet the provider's original clinical intent has been changed.

That makes semantic mapping failures particularly important because **technical success can hide clinical failure**.

A mature validation approach checks:

- source test code
- target LIS code
- test dictionary definition
- expected components
- downstream analyzer/workflow
- returned result components

---

## Patient Identity Integrity

An MRN is not merely a number.

The identifier also exists within a namespace or assigning authority.

```text
12345 / HOSPITAL_A / MR
```

is not automatically equivalent to:

```text
12345 / HOSPITAL_B / MR
```

Validation should preserve the relationship between:

```text
Patient
   ↓
Order
   ↓
Specimen / accession
   ↓
Result
```

An ambiguous or inconsistent relationship should follow a defined exception workflow rather than silently filing against an uncertain patient record.

---

## Critical Result Integrity

A result can be transmitted numerically correctly while losing clinically meaningful interpretation.

Example:

```text
LIS:
Potassium 6.8
Critical High

Expected interface:
HH

Received interpretation:
H
```

The numerical result survived.

The clinical meaning did not.

> **Important distinction:** Preserving a critical interpretation such as `HH` does not by itself prove that the organization's critical-result notification workflow occurred.

Interface data integrity and clinical escalation workflow are related but separate controls.

---

# Production Validation Mindset

Finding the likely fix is not the same as safely deploying it.

A production-oriented workflow should include:

```text
Reproduce defect
      ↓
Establish baseline
      ↓
Assess clinical/downstream risk
      ↓
Document configuration change
      ↓
Unit testing
      ↓
Integration testing
      ↓
Negative testing
      ↓
Regression testing
      ↓
Clinical/application signoff
      ↓
Controlled deployment
      ↓
Canary transactions
      ↓
Post-live verification
      ↓
Monitoring
```

Rollback criteria should be established before production change.

### Example validation cases

| Test | Expected outcome |
|---|---|
| Normal laboratory result | Correct patient, order, value and status |
| Critical result | Critical interpretation preserved |
| CBC with differential | Correct order mapping and all expected components |
| Ambiguous patient identifier | Routes to defined exception handling |
| Incorrect order/accession relationship | Does not silently auto-file |
| Invalid test mapping | Fails safely or routes for review |
| Existing unaffected tests | Continue working after change |
| Production canary | End-to-end behavior matches validation |

---

# Design Principles Demonstrated

### Find the last known-good state

Do not guess which application is responsible.

Trace the transaction until the first boundary where the information becomes incorrect.

### Preserve clinical meaning

A technically valid message can still represent the wrong patient, order, specimen, timestamp, interpretation, or test.

### Segment before optimizing

A system-wide average can hide one site, workflow, route, shift, or specimen population causing most of the deterioration.

### Contain before correcting

Protect the workflow first. Then establish root cause and implement the permanent fix.

### Validate the failure path

Testing only the happy path is insufficient for clinical systems.

Know what happens when identifiers conflict, mappings fail, messages are rejected, or downstream systems are unavailable.

### Measure the outcome after the change

A configuration change is not finished when it is deployed.

It is finished when post-change evidence demonstrates that the process remains reliable.

---

# About This Project

This is a **synthetic portfolio case study** designed to demonstrate clinical-systems reasoning using realistic laboratory and HL7 workflows.

No PHI or proprietary hospital data is used.

The operational scenarios and performance results are simulated and are intended to demonstrate an approach to:

- laboratory workflow investigation
- LIS/EHR interoperability
- interface validation
- clinical data integrity
- production change management
- operational decision-making

My laboratory background informs the clinical workflow and patient-safety perspective behind the project.

---

## Professional Focus

I am interested in work at the intersection of **clinical laboratory operations and healthcare technology**, including:

- LIS / laboratory systems
- clinical applications
- laboratory informatics
- interface and integration support
- instrument/middleware connectivity
- implementation and validation
- laboratory process improvement

**ASCP-certified Medical Laboratory Technician (MLT)** with technical and analytical experience applying laboratory domain knowledge to systems-oriented problems.

---

## Core Skills

`LIS` `HL7 v2` `EHR Interfaces` `ORU` `ACK` `Laboratory Workflow` `Clinical Applications` `Interface Validation` `Patient Identity` `Test Mapping` `Root Cause Analysis` `Change Control` `Regression Testing` `Healthcare IT`
