# 💻 AWS CLI — Hands-On Lab

## 🎯 Objective

Understand how the **AWS Command Line Interface (CLI)** works and practice accessing AWS services from a command-line environment.

---

## What I Practiced

* Understanding AWS Access Keys
* Understanding programmatic access
* Installing AWS CLI
* Checking the AWS CLI installation
* Configuring AWS CLI
* Setting the default AWS Region
* Running AWS CLI commands
* Understanding IAM authorization through the CLI

---

# What is AWS CLI?

The **AWS Command Line Interface** allows AWS services and resources to be accessed and managed using commands from a terminal.

```text
Terminal
   ↓
AWS CLI
   ↓
AWS API
   ↓
IAM Authorization
   ↓
AWS Service
```

---

## AWS Access Keys

Programmatic access can involve credentials consisting of:

```text
Access Key ID
      +
Secret Access Key
```

These credentials are sensitive and must be protected.

> ⚠️ **Never upload Access Key IDs or Secret Access Keys to a public GitHub repository.**

---

## Check AWS CLI Installation

The following command can be used to check the installed AWS CLI version:

```bash
aws --version
```

---

## Configure AWS CLI

AWS CLI can be configured using:

```bash
aws configure
```

Configuration can include:

```text
AWS Access Key ID
AWS Secret Access Key
Default Region
Default Output Format
```

Sensitive credential values should **never** be documented in this repository.

---

## Running AWS CLI Commands

Example:

```bash
aws iam list-users
```

This requests a list of IAM users.

The command succeeds only if the identity being used has the required IAM permissions.

---

## CLI Does Not Bypass IAM

Using AWS CLI does not provide unrestricted access.

```text
CLI Command
    ↓
AWS API Request
    ↓
IAM Permission Check
    ↓
Allowed / Denied
```

The configured identity must still be authorized to perform the requested action.

---

# AWS SDK

AWS also provides **Software Development Kits (SDKs)** that allow applications to communicate with AWS services programmatically.

Conceptually:

```text
Application
     ↓
AWS SDK
     ↓
AWS APIs
     ↓
AWS Services
```

SDKs are available for multiple programming languages.

---

## 🔐 Security Connection

AWS CLI introduces an important cybersecurity topic:

### Credential Management

Access keys are credentials and must be protected from unauthorized access.

Exposed AWS credentials can potentially allow an attacker to interact with AWS resources according to the permissions associated with those credentials.

---

## ⚠️ GitHub Security

Never commit files containing AWS credentials.

Examples of information that should **not** be published:

```text
Access Key ID
Secret Access Key
Passwords
Authentication tokens
Private credentials
```

Before pushing AWS projects to GitHub, always check that no credentials are present.

---

## 🔒 Security Best Practices

* Never hard-code AWS credentials in source code.
* Never publish credentials on GitHub.
* Protect Secret Access Keys.
* Follow least privilege for programmatic identities.
* Remove credentials that are no longer required.
* Avoid creating unnecessary access keys.
* Use secure credential-management approaches for real environments.

---
