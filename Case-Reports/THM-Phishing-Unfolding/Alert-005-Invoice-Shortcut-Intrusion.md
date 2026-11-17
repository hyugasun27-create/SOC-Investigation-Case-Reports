# Case Report 005 — Malicious Invoice Shortcut Execution Chain

## 1. Case Overview

| Field | Details |
|---|---|
| Training platform | TryHackMe |
| Room | Phishing Unfolding |
| Investigation type | Email attachment, endpoint, and network investigation |
| Host | `win-3450` |
| Classification | **True Positive — malicious execution observed** |
| Impact status | Remote shell and reconnaissance observed; broader impact not fully established |

## 2. Initial Email and File Evidence

| Field | Observed value |
|---|---|
| Sender | `john@hatmakereurope.xyz` |
| Attachment | `ImportantInvoice-Febrary.zip` |
| Extracted items | `ImportantInvoice-Febrary`, `invioce.pdf.lnk` |

Outlook cache records showed the archive arriving at approximately 12:24:35 UTC. Explorer activity involving the suspicious shortcut was observed at approximately 12:24:46.

The double extension and shortcut format are important context: the item appeared invoice-related but was a Windows shortcut rather than an ordinary PDF document.

## 3. Execution Evidence

Sysmon process creation telemetry showed:

| Field | Observed value |
|---|---|
| Host | `win-3450` |
| Parent process | `explorer.exe` |
| Parent PID | `3180` |
| Child process | `powershell.exe` |
| Child PID | `3880` |
| Approximate execution time | 12:24:47 UTC |

The PowerShell process launched by Explorer downloaded and executed Powercat.

## 4. Network and Reconnaissance Evidence

At approximately 12:24:50 UTC, the endpoint established a reverse-shell connection to:

- Host: `2.tcp.ngrok.io`
- Port: `19282`

The subsequent shell activity included reconnaissance commands:

- `whoami`
- `systeminfo`
- `net user`
- `net localgroup`

The sequence of a suspicious shortcut, PowerShell execution, Powercat use, reverse-shell connection, and host/user enumeration supports a malicious execution chain.

## 5. Attack Timeline

| Approx. time (UTC) | Activity |
|---|---|
| 12:24:35 | Invoice ZIP observed in Outlook cache |
| 12:24:46 | Explorer activity involving suspicious shortcut |
| 12:24:47 | Explorer launched PowerShell (PID 3880) |
| 12:24:50 | Reverse shell to `2.tcp.ngrok.io:19282` |
| After connection | Host and user reconnaissance commands executed |

Times are approximate and based on the lab telemetry summarized during investigation.

## 6. Classification Rationale

**True Positive — malicious execution observed.**

The evidence goes beyond a suspicious email: a shortcut was opened, PowerShell executed Powercat, a reverse-shell connection was established, and reconnaissance commands were run. This demonstrates malicious code execution and remote access activity on the endpoint.

The evidence summarized here does not independently establish the full extent of data access, persistence, lateral movement, or exfiltration.

## 7. Recommended SOC Actions

1. Isolate the affected endpoint from the network according to incident-response policy.
2. Preserve the email, archive, shortcut, Sysmon logs, Windows event logs, and relevant network capture.
3. Block or investigate the observed remote endpoint and related infrastructure.
4. Review PowerShell and process telemetry for follow-on commands, payloads, persistence, and lateral movement.
5. Identify any credentials exposed during the session and follow the organization's credential-reset procedure.
6. Document all response actions and maintain evidence integrity.

## 8. Evidence Limitations

The exact PowerShell command line, Powercat payload hash, persistence evidence, and full command-and-control infrastructure details were not included in the evidence summary used for this report. These should be added only after validating the original logs and artifacts.

## 9. Final Conclusion

The invoice-themed attachment and shortcut led to PowerShell execution, Powercat use, and a reverse-shell connection followed by host and user reconnaissance. This is a **confirmed malicious execution event** in the simulated lab environment. The complete scope and impact remain to be determined from the remaining artifacts.

---

*Training-lab report. The findings describe a simulated incident and must not be interpreted as a real-world compromise.*
