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
