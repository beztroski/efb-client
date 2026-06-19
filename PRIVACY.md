# PRIVACY POLICY — ESCAPE FROM BRILSK CLIENT

**Effective Date:** [19/06/2026]
**Last Updated:** [19/06/2026]

This Privacy Policy describes how the "Escape From Brilsk" project ("Project," "we," "us") processes information when you use our official client and related services. Our primary goal is to maintain a fair, secure, and abuse-free environment. This Policy is incorporated into our End User License Agreement.

## 1. DATA WE PROCESS
We only process the minimum technical identifiers necessary to enforce our license terms and protect the integrity of the service. We do not require your real name, email address, or physical location.

### (a) Telegram User ID
A unique numeric identifier obtained when you register through our official bot. Used as your account key for authentication and support.

### (b) Hardware Fingerprint (HWID)
A non-reversible, one-way hash generated from a subset of your device's hardware characteristics. Standing alone, this does not identify you as an individual. It is used solely to prevent multi-account abuse and ban evasion by binding a license to a specific machine.

### (c) IP Address
- **Transient (clear text):** When you connect to our services, we temporarily process your IP address in its original form for a maximum of **7 days**. This is used for operational security, rate-limiting, and DDoS mitigation.
- **Persistent (hashed):** After 7 days, the IP address is irreversibly hashed using a strong cryptographic algorithm. The resulting **hash is retained indefinitely** as part of a security blacklist. The hashed form does not allow us to know the original IP, but it enables us to detect and block known malicious actors or users who attempt to circumvent bans. The original clear-text IP is deleted upon hashing.

### (d) Client Diagnostics
Includes client version, modpack version, and anonymized crash logs. Used exclusively for debugging and ensuring compatibility.

## 2. HOW WE USE YOUR DATA (PURPOSES AND LEGAL BASES)
- **Contractual Necessity:** To authenticate your account, deliver the Licensed Application, and enforce the EULA (binding your Telegram ID and HWID).
- **Legitimate Interests:** To safeguard the security and stability of our client, prevent fraud, enforce bans, and maintain a blacklist of hashed identifiers. Our interests are balanced against your rights, given the non-sensitive nature of the data and our strong security practices.
- **Consent (where required):** For users in jurisdictions that require explicit consent for hashed IP retention, your consent is signaled by the affirmative act of using the client after being presented with this Policy. If you disagree, you must not use the client.

## 3. DATA RETENTION
- **Telegram ID & HWID:** Stored for the active lifecycle of your account and for a reasonable period thereafter to enforce bans.
- **Clear-text IP:** Automatically deleted after **7 days**.
- **Hashed IP:** Stored permanently in our security blacklist. Because the hash is one-way and cannot be reversed to the original IP, this retention is strictly for security enforcement and is exempt from erasure requests under applicable data protection laws that recognize this practice as a legitimate safeguard.
- **Crash logs:** Retained for a short rolling period and then permanently deleted.

## 4. DATA SHARING AND SECURITY
We do **not** sell, trade, or monetize your data. We may disclose information only: (a) to infrastructure providers necessary for hosting (bound by confidentiality); or (b) when compelled by a valid legal order. We implement technical and organizational measures to protect all identifiers, including encryption at rest for hashed IP lists and strict access controls.

## 5. YOUR RIGHTS AND CONTACT
Depending on your jurisdiction, you may have rights to access, rectify, or delete your personal data. Because we operate with a lightweight, privacy-focused architecture, we rely on automated processes. To make a verifiable request, please contact us through the official communication channels listed in our repository, including from the Telegram account you used to register. We will respond within statutory timeframes. Note that we may decline requests that would impair our ability to enforce bans, including deletion of hashed IPs that constitute a necessary security measure.

## 6. UPDATES
We may revise this Privacy Policy from time to time. Significant changes will be announced via our official Telegram channel. Continued use of the client after changes constitutes acceptance of the updated Policy.
