# Case Report 003 — WUDFHost.exe Process Validation

## 1. Case Overview

| Field | Details |
|---|---|
| Training platform | TryHackMe |
| Room | Phishing Unfolding |
| Investigation type | Endpoint process triage |
| Lab event time | 2026-11-17 12:08:35.229 (UI timestamp) |
| Host | `win-3455` |
| Classification | **Likely legitimate process / no malicious activity demonstrated** |

## 2. Process Event

| Field | Observed value |
|---|---|
| Data source | Sysmon |
| Event ID | 1 — Process Create |
| Process | `WUDFHost.exe` |
| PID | `3809` |
| Parent process | `services.exe` |
| Parent PID | `3648` |
| Image path checked | `C:\Windows\System32\WUDFHost.exe` |
| File size | 256,512 bytes |
| Authenticode status | Valid |
| Signer | Microsoft Windows |

SHA-256:
`2E9EDAC8AD372BD9ADFFE852A18F5FBD25DE804C0CA39C2F5B88365CC443C138`

## 3. Investigation and Findings

The process was checked using its file path, parent process, digital signature, file metadata, and hash.

Observed findings:

- The executable was located in the expected Windows System32 directory.
- The parent process was `services.exe`, consistent with a Windows service-hosted device framework process.
- Authenticode validation succeeded and identified Microsoft Windows as the signer.
- The observed command line used typical UMDF host arguments.

These checks support the legitimacy of the binary. A valid signature alone does not prove that every execution is benign, so surrounding process and network activity should still be considered.

## 4. Classification Rationale

**Likely legitimate — no malicious behaviour demonstrated by the available event.**

The expected path, Microsoft signature, and service parent process support a benign assessment. Close as a false positive only if surrounding telemetry contains no contradictory indicators.

## 5. Recommended SOC Actions

1. Retain the process event and hash in the investigation notes.
2. Check for unusual child processes, unexpected network connections, or execution from alternate paths if additional alerts exist.
3. Reopen the investigation if new evidence contradicts the legitimacy assessment.

## 6. Final Conclusion

The available evidence supports a legitimate Windows `WUDFHost.exe` execution. No malicious behaviour was demonstrated in the event and file-validation checks performed.

---

*Training-lab report. File authenticity and observed process context are evidence, not an absolute guarantee of benign behaviour.*
