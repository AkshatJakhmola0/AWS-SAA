# 🔐 IAM Multi-Factor Authentication (MFA) — Hands-On Lab

## 🎯 Objective

Understand and practice **Multi-Factor Authentication (MFA)** to improve the security of AWS identities.

---

## What is MFA?

MFA adds another authentication factor in addition to a password.

Without MFA:

```text
Username
    +
Password
```

With MFA:

```text
Username
    +
Password
    +
MFA
```

This provides additional protection if a password becomes compromised.

---

## Why MFA Matters

Passwords can potentially be:

* Stolen
* Guessed
* Reused
* Exposed through phishing
* Leaked in a data breach

With MFA enabled, possession of the password alone may not be enough to authenticate successfully.

---

## 🧪 Hands-On Steps

### Step 1 — Open Security Credentials

Navigated to the security settings for the AWS identity.

### Step 2 — Configure MFA

Selected and configured an MFA method.

### Step 3 — Register MFA

Completed the required MFA registration process.

### Step 4 — Verify MFA

Verified that the MFA device was configured correctly.

### Step 5 — Test Authentication

Used the configured authentication process to understand how MFA adds another layer of account protection.

---

## Root Account Security

MFA is especially important for the AWS root account because the root user has extremely powerful access to the AWS account.

```text
AWS Root Account
       │
       ├── Enable MFA
       ├── Protect credentials
       ├── Avoid everyday use
       └── Avoid unnecessary access keys
```

---

## 🔐 Cybersecurity Connection

MFA demonstrates the concept of **defense in depth**.

Instead of relying on one security control:

```text
Password
```

multiple controls can be combined:

```text
Password
   +
MFA
   +
IAM Permissions
   +
Monitoring
```

Compromise of one control therefore does not necessarily mean complete compromise of the AWS environment.

---

## 🔒 Security Best Practices

* Enable MFA for the root account.
* Use MFA for privileged identities.
* Protect MFA devices.
* Avoid using root for everyday activities.
* Combine MFA with least-privilege permissions.
* Protect account recovery mechanisms.

---

