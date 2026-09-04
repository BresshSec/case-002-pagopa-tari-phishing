# Case 002 — PagoPA / TARI Refund Phishing Campaign

> Independent threat research case study focused on a phishing campaign targeting Italian users through a fake TARI refund impersonating PagoPA.

## Overview

This case study documents an independent analysis of a phishing campaign impersonating **PagoPA** and using a fake **TARI refund** as a lure against Italian users.

The campaign had already been identified and publicly reported by **CERT-AGID**. The objective of this research is therefore **not to claim discovery of the campaign**, but to document an independent investigation of one observed phishing instance and the methodology used to analyze and correlate its infrastructure.

The investigation started from the following phishing landing page:

`hxxps://rimborso[.]intp[.]cam/it`

From this initial indicator, the analysis included:

- controlled sandbox analysis of the phishing flow;
- extraction of network and application indicators;
- DNS and passive DNS investigation;
- infrastructure pivoting and correlation;
- identification of related phishing hostnames;
- analysis of frontend/application artifacts;
- comparison with CERT-AGID's publicly released IoCs.

Several infrastructure relationships identified during the investigation were subsequently confirmed in the IoC dataset published by CERT-AGID.

---

## Case Information

| Field | Value |
|---|---|
| Case | 002 |
| Analysis date | 4 September 2026 |
| Campaign | PagoPA / TARI refund phishing |
| Target | Italian users |
| Impersonated entity | PagoPA S.p.A. |
| Lure | Refund for an alleged TARI overpayment |
| Initial indicator | `rimborso[.]intp[.]cam` |
| Analysis type | Phishing / OSINT / Infrastructure correlation |
| Status | Confirmed phishing |
| External validation | CERT-AGID |

---

## Research Scope

The investigation was conducted using publicly available threat intelligence sources and controlled sandbox environments.

No real personal, authentication, banking or payment information was submitted to the phishing infrastructure.

Infrastructure correlations documented in this repository should not be interpreted as threat-actor attribution.

The identification of a hosting provider, ASN, registrar or other infrastructure provider does **not** imply involvement by that provider in the malicious activity.

### Active Scanning

No active port scanning, vulnerability scanning, directory brute-forcing, exploitation or unauthorized access was performed against the identified infrastructure.

Active service enumeration was intentionally excluded because the infrastructure was outside the researcher's control and may be shared by unrelated tenants.

The investigation therefore remained limited to passive OSINT, public threat-intelligence data and observations obtained through controlled sandbox execution.

---

## Investigation Workflow

```text
Initial phishing URL
        │
        ▼
Sandbox analysis
        │
        ▼
Phishing flow reconstruction
        │
        ▼
DNS / Network indicators
        │
        ▼
Passive DNS pivoting
        │
        ▼
Related infrastructure
        │
        ▼
Application fingerprinting
        │
        ▼
CERT-AGID IoC correlation

## Analysis

### Phishing Flow Analysis

The phishing website was analyzed inside a controlled **ANY.RUN sandbox**.

The observed landing page impersonated **PagoPA** and presented the victim with a notification regarding an alleged **TARI overpayment**.

The phishing flow was structured as a multi-stage process designed to progressively collect information from the victim.

#### Stage 1 — Fake TARI Refund Notice

The initial landing page displayed a refund notification related to an alleged overpayment of the Italian waste tax (TARI).

Observed elements included:

- PagoPA branding and visual identity;
- reference to a TARI overpayment;
- a fake practice/reference number;
- a call-to-action inviting the victim to continue the refund request.

Observed practice number:

`PAG/2026/TARI-042`

The main call-to-action displayed:

`Continua con la richiesta`

---

#### Stage 2 — Identity Verification

After continuing, the phishing application displayed a page titled:

`Consultazione pratica di rimborso TARI`

The victim was asked to verify their identity using either:

- Italian tax code (*Codice Fiscale*);
- identity card (*Carta d'identità*).

This stage indicates that the campaign was designed to collect personally identifiable information before progressing further through the phishing workflow.

Only fictitious test data was used during sandbox interaction.

---

#### Stage 3 — Fake Refund Approval

After the identity verification step, the application displayed a fake confirmation indicating that the refund had been approved.

The following information was observed:

| Field | Observed value |
|---|---|
| Refund amount | `95,00 EUR` |
| Status | `Rimborso approvato` |
| Verification | `Verifica completata` |
| Tax | `TARI - Tassa sui Rifiuti` |
| Reference year | `2025` |
| Alleged overpayment date | `29/08/2026` |

The interface then presented another call-to-action:

`Procedi con la richiesta`

The progressive structure of the workflow appears designed to increase legitimacy before requesting additional information from the victim.

---

#### Observed Phishing Flow

```text
Fake PagoPA landing page
        │
        ▼
TARI overpayment notification
        │
        ▼
"Continua con la richiesta"
        │
        ▼
Identity verification
(Codice Fiscale / Carta d'identità)
        │
        ▼
Fake verification completed
        │
        ▼
95 EUR refund approved
        │
        ▼
"Procedi con la richiesta"
        │
        ▼
Further personal / payment data collection
```

The investigation did not require the submission of real personal, authentication, banking or payment information.

The later stages of the campaign were subsequently compared with the publicly documented CERT-AGID analysis and IoC dataset.

---

### Initial Network Findings

Network activity observed during sandbox execution identified the following infrastructure:

| Indicator | Type | Observation |
|---|---|---|
| `rimborso[.]intp[.]cam` | Domain | Initial phishing host |
| `intp[.]cam` | Domain | Parent domain |
| `170[.]106[.]154[.]175` | IPv4 | Resolved phishing host |
| `AS132203` | ASN | ASN associated with the observed IP |

DNS resolution observed during the investigation:

```text
rimborso.intp.cam → 170.106.154.175
```

HTTP traffic showed the phishing application being served over HTTPS after an HTTP redirect.

During sandbox execution, unrelated browser and operating-system traffic was also generated. These requests were excluded from the IoC set unless an explicit relationship with the phishing application could be established.

> **Attribution note:** The ASN and hosting infrastructure observations identify infrastructure associated with the analyzed service. They do not identify the threat actor and do not imply involvement by the infrastructure provider.

---

### Sandbox Detection

During execution, the sandbox classified the analyzed activity as phishing.

A network detection was also generated:

`PHISHING has been detected (SURICATA)`

The analysis showed the phishing application communicating primarily with its own infrastructure while the sandbox environment generated additional legitimate browser and operating-system traffic.

These unrelated requests were treated as environmental noise and were not considered campaign indicators.
