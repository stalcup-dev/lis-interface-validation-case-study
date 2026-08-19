# APCM Revenue Integrity Case Study

**Independent RCM problem-solving demonstration by Allen Stalcup**

**Revenue Cycle Management | Revenue Integrity | Medicare APCM | Healthcare Operations**

[**View the Full Case Study (PDF)**](./Allen_Stalcup_APCM_Revenue_Integrity_Case_Study_Portfolio.pdf)

---

## Executive Summary

This portfolio case study demonstrates how I would investigate a current **Advanced Primary Care Management (APCM)** billing concern from a Revenue Cycle Management and revenue integrity perspective.

The scenario was prompted by public reporting involving Medicare patients who reported receiving recurring APCM-related cost-sharing charges they did not clearly understand or remember authorizing.

Rather than assume the charges were correct or incorrect, I approached the issue as an RCM analyst would:

> **Determine what should have happened, test what actually happened, quantify the variance, identify the root cause, and design controls that prevent recurrence.**

The result is an evidence-driven investigation framework connecting:

**Clinical operations -> documentation -> patient consent -> coding -> claims -> payer adjudication -> patient responsibility -> compliance risk -> financial impact**

## The Business Problem

APCM allows eligible primary care practices to receive monthly Medicare reimbursement for ongoing patient-management activities performed outside of traditional office visits.

The billing model creates an important revenue opportunity for primary care organizations, but it also introduces operational risk.

A technically payable claim may still require investigation if:

- Patient consent cannot be demonstrated
- APCM is billed after a patient opts out
- The wrong APCM level is selected
- Multiple APCM claims are submitted for the same patient-month
- Eligibility requirements are not supported
- Patient responsibility is calculated incorrectly
- QMB protections are not applied correctly
- Clinical, billing, and enrollment systems fall out of sync

The central revenue-integrity question becomes:

> **Is the revenue earned, supported, compliant, collectible, and defensible?**

## North Star

### Decision

Determine whether APCM revenue is properly supported and defensible.

### Threshold

Identify patient-months failing eligibility, consent, coding, opt-out, duplicate-billing, or patient-balance controls.

### Action

Correct affected accounts, quantify financial exposure, identify systemic root causes, and implement controls that prevent recurrence.

## How I Would Investigate the Issue

### 1. Define the affected population

Extract all APCM patient-months within the review period, including:

- Patient identifier
- Date of service
- Service month
- APCM HCPCS code
- Rendering / billing provider
- Payer
- Charge amount
- Allowed amount
- Medicare payment
- Secondary payment
- Patient responsibility
- Claim status
- Practice location

### 2. Reconcile claims to clinical and enrollment evidence

Join claims against relevant EHR and eligibility information:

- APCM consent date
- Consent method
- Opt-out date
- Chronic-condition status
- QMB status
- Responsible PCP
- Enrollment status
- Required clinical documentation
- Relevant initiating visit information

This creates a patient-month view that connects **what was billed** with **the evidence supporting the bill**.

### 3. Run exception tests

The analysis would flag conditions such as:

| Exception Test | Question |
|---|---|
| `consent_missing` | Was APCM billed without documented consent? |
| `consent_after_claim` | Was the first APCM claim submitted before consent was captured? |
| `billed_after_opt_out` | Was APCM billed after the patient discontinued the service? |
| `duplicate_patient_month` | Was more than one APCM service billed for the same patient-month? |
| `incorrect_apcm_level` | Does the billed APCM level match documented patient eligibility? |
| `qmb_patient_balance` | Was prohibited Medicare cost sharing assigned to a QMB beneficiary? |
| `eligibility_exception` | Does the record fail required eligibility or documentation criteria? |

### 4. Quantify exposure

Exceptions would then be translated into decision-ready metrics:

- Total APCM patient-months reviewed
- Clean patient-month rate
- Exception rate
- Claims requiring coding review
- Claims requiring refund / adjustment review
- Patient-balance exposure
- Gross allowed-amount exposure
- Exception concentration by provider or location
- Root-cause concentration

This moves the conversation from:

> "Some patients complained about a bill."

To:

> **"Here is the size, pattern, financial exposure, and likely cause of the control failure."**

### 5. Identify the root cause

The goal is not simply to correct individual claims.

The stronger question is:

> **Why was the system capable of producing the exception?**

Potential root causes could include:

- Consent workflow disconnected from billing
- Enrollment data not synchronized with the EHR
- Opt-out status not communicated to the billing system
- Manual coding variation
- Eligibility logic applied inconsistently
- QMB status not checked before patient statements
- Inadequate exception reporting
- Patient communication that does not clearly explain recurring cost sharing

### 6. Install controls

A mature response includes **preventive, detective, and corrective controls**.

#### Preventive controls

- Block APCM claim generation when required consent is absent
- Prevent billing after an effective opt-out date
- Validate eligibility before code assignment
- Validate QMB status before patient responsibility is generated

#### Detective controls

- Monthly APCM exception report
- Duplicate patient-month monitoring
- Consent-to-claim reconciliation
- Provider/location exception trending
- Patient-balance audits

#### Corrective controls

- Claim adjustment workflow
- Patient balance correction
- Refund review
- Coding/compliance escalation
- Root-cause remediation
- Staff education when workflow failures are identified

## Executive Output

The final product should not be a spreadsheet full of exceptions.

Leadership needs a decision pack answering:

**What happened?**

**How widespread is it?**

**What is the financial and compliance exposure?**

**Why did it happen?**

**What should we do now?**

**What control prevents it from happening again?**

The accompanying case study demonstrates how I would translate detailed RCM analysis into that executive-level output.

### [Open the Full APCM Revenue Integrity Case Study](./Allen_Stalcup_APCM_Revenue_Integrity_Case_Study_Portfolio.pdf)

## Capabilities Demonstrated

This case study is designed to demonstrate practical capability across:

- **Revenue Cycle Management**
- **Revenue Integrity**
- **Medicare billing analysis**
- **Claims-to-clinical reconciliation**
- **Healthcare data analysis**
- **Patient financial responsibility**
- **Billing and coding controls**
- **Compliance-oriented exception testing**
- **Root-cause analysis**
- **Financial-risk quantification**
- **Operational process improvement**
- **Executive communication**
- **Control design and monitoring**

The objective is not simply to identify unusual data.

It is to convert healthcare revenue-cycle data into **actionable operational decisions**.

## Why This Case Matters

Healthcare revenue problems rarely exist entirely inside the billing department.

A single recurring charge can involve:

**Patient experience -> Clinical workflow -> Documentation -> Consent -> Coding -> Claims -> Payer rules -> Patient responsibility -> Compliance -> Cash**

Strong revenue-cycle analysis requires understanding those relationships rather than treating claims as isolated transactions.

This case demonstrates that end-to-end perspective.

## Portfolio Disclaimer

This is an **independent portfolio case study created by Allen Stalcup to demonstrate how I would approach and solve a real-world Revenue Cycle Management problem**.

The scenario was inspired by publicly available reporting and Medicare guidance.

I am not affiliated with, employed by, or acting on behalf of the healthcare organization discussed in the source reporting. I did not access the organization's claims systems, EHR, internal records, financial information, or patient information.

**No protected health information (PHI) is included.**

Any patient-level records, exception counts, financial figures, operational findings, or analytical results used to demonstrate the methodology are **synthetic and illustrative**.

The case study does **not** make a finding that any specific healthcare organization violated Medicare requirements or engaged in improper billing.

Its purpose is to demonstrate the analytical framework I would use to determine the facts.

**Case-study snapshot: August 2026**

## Primary Reference Framework

The analysis is grounded primarily in:

- Centers for Medicare & Medicaid Services guidance for **Advanced Primary Care Management Services**
- Medicare guidance concerning **Qualified Medicare Beneficiary (QMB)** cost-sharing protections
- Public reporting describing the patient-billing scenario used to frame the case

Regulatory requirements and payer guidance can change over time. Production analysis should always validate requirements against the guidance applicable to the dates of service under review.

## About the Author

### Allen Stalcup

I bring a combination of **clinical healthcare experience, Revenue Cycle Management knowledge, and analytical problem-solving**.

My background includes outpatient healthcare experience, ASCP Medical Laboratory Technician certification, and formal Revenue Cycle, Billing & Coding education.

I am particularly interested in healthcare roles where domain knowledge and systems thinking can improve:

- Revenue integrity
- Clinical operations
- Healthcare applications and workflows
- Quality and compliance
- Billing operations
- Process improvement
- Patient financial experience

My approach is simple:

> **Understand the operational problem first. Use data to determine what is happening. Then build the smallest reliable control that improves the decision.**

## Repository Contents

```text
.
|-- README.md
`-- Allen_Stalcup_APCM_Revenue_Integrity_Case_Study_Portfolio.pdf
```

The PDF contains the complete investigation framework, illustrative analytical workup, control recommendations, remediation roadmap, and executive decision structure.

## Recruiter / Hiring Manager Note

This project is intended to show more than familiarity with healthcare data.

It demonstrates how I would enter an ambiguous, current healthcare revenue problem and structure it into:

**Rule -> Evidence -> Test -> Exception -> Financial Impact -> Root Cause -> Control -> Decision**

That is the standard I aim to bring to healthcare operations and revenue-cycle work.
