# Case Report 001 — Inheritance-Themed Phishing Email

## 1. Case Overview

| Field | Details |
|---|---|
| Training platform | TryHackMe |
| Room | Phishing Unfolding |
| Investigation type | Email security / SOC alert triage |
| Alert time | 2026-11-17 11:54:33.229 (lab UI timestamp) |
| Classification | **True Positive — phishing attempt** |
| Outcome | Phishing attempt identified; successful compromise not established |

## 2. Alert Details

| Field | Observed value |
|---|---|
| Data source | Email |
| Direction | Inbound |
| Sender | `eileen@trendymillineryco.me` |
| Recipient | `support@tryhatme.com` |
| Subject | Inheritance Alert: Unknown Billionaire Relative Left You Their Hat Fortunes |
| Attachment | None |

The message claimed that a long-lost billionaire relative had left the recipient a hat-related fortune and requested banking details urgently.

## 3. Initial Assessment

The message was treated as a suspected phishing attempt because it combined an unexpected inheritance/wealth lure with an urgent request for sensitive financial information.

## 4. Evidence and Analysis

Observed indicators:

- Unexpected inheritance and large financial reward claim.
- Urgent request for banking details.
- No verifiable details about the alleged relative or inheritance were provided in the event content.
- No attachment was present.

The absence of an attachment does not make the message safe. Phishing can seek information directly through a reply or a later interaction rather than through a malicious file.

## 5. Classification Rationale

**True Positive — phishing attempt.**

The content is consistent with financial-information harvesting through social engineering. The request for banking details is the strongest indicator.

This classification means the message itself is a phishing attempt. It does **not** establish that the recipient replied, disclosed banking information, or suffered a compromise.

## 6. Recommended SOC Actions

1. Preserve the original email and full headers for further analysis.
2. Review sender infrastructure and authentication results where available.
3. Contact the intended recipient and advise them not to respond or disclose financial information.
4. Search the environment for related messages or interactions.
5. If financial information was disclosed, escalate immediately through the organization's incident-response process.

## 7. Evidence Limitations

The available event did not establish sender IP reputation, SPF/DKIM/DMARC results, recipient interaction, or successful data disclosure. These should not be assumed.

## 8. Final Conclusion

The inbound inheritance-themed email is a **True Positive phishing attempt** based on its urgent request for banking details and deceptive financial-inheritance lure. No evidence in the available event confirms successful credential or financial-data theft.

---

*Training-lab case report. Findings are limited to the evidence observed in the lab event. No real-world user or organization is represented.*
