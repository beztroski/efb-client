# SECURITY POLICY — ESCAPE FROM BRILSK CLIENT

**Last Updated:** [Date]
**Scope:** This document describes our technical security posture, data protection measures, and responsible disclosure procedures for the Escape From Brilsk project.

---

## 1. PHILOSOPHY AND DESIGN PRINCIPLES
The Escape From Brilsk client is architected with a "privacy by design" approach. We recognise that our enforcement of license integrity requires the processing of certain technical identifiers, and we accept the corresponding duty to protect those identifiers with industry-standard safeguards. Every identifier we collect is subject to the principle of least privilege: it is used for a single, defined purpose and retained no longer than technically necessary for that purpose — except where cryptographic hashing renders it irreversibly anonymised, at which point it serves exclusively as a security control.

---

## 2. IDENTIFIER PROTECTION ARCHITECTURE

### 2.1 Hardware Fingerprint (HWID)
- **Generation:** The HWID is derived client-side by sampling multiple hardware and system parameters. The raw parameters are concatenated, salted with a project-specific static salt, and passed through a one-way SHA-256 hashing function. At no point are the raw hardware values transmitted to our servers.
- **Transmission:** The resulting hash is transmitted over an encrypted TLS 1.3 channel.
- **Storage:** HWID hashes are stored at rest in a hardened database protected by filesystem-level encryption and strict access control lists. Only the authentication subsystem — segregated from public-facing services — has read access.

### 2.2 IP Address
We apply a bifurcated retention model designed to reconcile operational necessity with long-term privacy:

| Phase | Form | Retention | Purpose |
|:---|:---|:---|:---|
| **Operational** | Original (clear-text) | **7 days** from time of last connection | Rate-limiting, DDoS detection, active session security, and connection diagnostics |
| **Persistent** | Irreversible hash (SHA-256 with per-IP salt) | **Indefinite** | Blacklist enforcement; the hash is stored in a deny-list table that prevents re-registration or access by known malicious actors |

- **Hashing procedure:** At the 7-day boundary, a scheduled automated process salts the IP with a cryptographically secure random value, hashes it, and **immediately and irretrievably deletes** the original clear-text entry. The hashing salt is stored separately from the hash table in a hardware security module (HSM) or equivalent key-vault service, such that a compromise of the database alone does not permit IP re-identification.
- **Immutability of the blacklist:** Once an IP is hashed and placed on the deny-list, the record is append-only. There is no mechanism to reverse the hash, and no API endpoint that accepts a clear-text IP and returns a match against the blacklist. Verification occurs internally during connection attempts: the incoming IP is salted with the same secret, hashed, and compared against the existing set of hashes. If a collision is found, the connection is refused. This process never logs the incoming IP in clear text as part of the blacklist check.

### 2.3 Telegram User ID
- **Transmission:** Obtained exclusively through the official Telegram Bot API over encrypted channels. We do not receive, request, or store your Telegram username, phone number, or message history.
- **Storage:** Stored as an integer in the authentication database, accessible only to the licence-validation microservice. This database resides in an isolated virtual private cloud (VPC) subnet with no direct internet ingress.

### 2.4 Client Diagnostics
- Crash logs and error traces are stripped of any IP address, HWID, or Telegram ID before being written to disk.
- Logs are retained on a rolling 14-day window and then permanently purged. Access to log storage is limited to two named maintainers for debugging purposes only.

---

## 3. TRANSMISSION AND NETWORK SECURITY
- All client-server communication is conducted exclusively over **TLS 1.3**, with TLS 1.2 supported solely for legacy compatibility on older Windows installations. We enforce strict cipher suites and have disabled all known weak protocols (SSLv3, TLS 1.0/1.1).
- Our API endpoints do not accept unencrypted HTTP connections; plain-text requests are met with a connection reset.
- Certificate pinning is implemented in the client to mitigate man-in-the-middle attacks against compromised certificate authorities.

---

## 4. ACCESS CONTROL AND INFRASTRUCTURE
- **Principle of least privilege:** No single service account has access to all data stores. The authentication system, crash-log aggregator, and blacklist verifier operate under distinct, narrowly scoped credentials with no cross-read permissions.
- **Multi-factor authentication (MFA)** is enforced for all maintainer accounts on infrastructure providers, code repositories, and administrative panels.
- Database backups are encrypted at rest using AES-256-GCM and stored in geographically redundant but access-restricted cold storage. Backup retention is limited to 30 days.
- All administrative access is logged, and audit logs are shipped to an append-only, tamper-evident logging service that is monitored for anomalous queries.

---

## 5. INCIDENT RESPONSE AND BREACH CONTAINMENT
In the unlikely event of a data exposure affecting technical identifiers:
1. **Immediate containment:** The affected subsystem is isolated from the network within minutes of detection.
2. **Root cause analysis:** Logs are preserved and analysed to determine the scope and vector.
3. **User notification:** Affected users will be notified via our official Telegram channel within 72 hours of confirmation. Because we hold no email addresses, this broadcast is the primary notification mechanism.
4. **Hardening:** Mitigations are deployed and verified before the affected service is restored. A post-mortem summary is published for transparency, omitting specific exploit details until a reasonable patching window has elapsed.

---

## 6. VULNERABILITY DISCLOSURE (RESPONSIBLE REPORTING)
We welcome security researchers and community members to report suspected vulnerabilities in the client, our infrastructure, or our privacy implementation. We ask that you:

- **Do not** exploit the vulnerability for any purpose beyond demonstrating its existence.
- **Do not** publicly disclose the vulnerability before we have had an opportunity to investigate and, if necessary, deploy a patch.
- Submit findings via our official Telegram channel or through the contact methods listed in our repository, using encrypted communication where possible.

We commit to:
- Acknowledging receipt within **5 business days**.
- Providing a realistic timeline for investigation and remediation.
- Publicly crediting the reporter upon resolution (unless anonymity is requested).
- Not pursuing legal action against individuals who report vulnerabilities in good faith and in accordance with this policy.

---

## 7. COMPLIANCE AND AUDIT
This Security Policy is reviewed on a biannual basis and updated to reflect evolving threats, infrastructure changes, and applicable regulatory requirements. While we are an independent project operating without a dedicated compliance team, we hold ourselves to the technical standards described herein. Where a provision of this Policy conflicts with a mandatory provision of applicable law, the law shall prevail, and we will amend this Policy accordingly.

---

*By maintaining this Security Policy, we aim to provide transparency into our protective measures without compromising the operational secrecy required to keep those measures effective.*
