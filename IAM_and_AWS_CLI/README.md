# 🔐 IAM and AWS CLI

This section documents my learning and hands-on practice with **AWS Identity and Access Management (IAM)** and the **AWS Command Line Interface (CLI)** while preparing for the **AWS Certified Solutions Architect – Associate (SAA-C03)**.

---

## 📚 Topics Covered

* IAM Users
* IAM Groups
* IAM Policies
* IAM Permissions
* Multi-Factor Authentication (MFA)
* AWS Access Keys
* AWS CLI
* AWS SDK
* AWS CLI Configuration
* Hands-on IAM and CLI practice

---

# 👤 AWS IAM

**AWS Identity and Access Management (IAM)** is used to control authentication and authorization for AWS resources.

IAM helps answer two important questions:

* **Who can access AWS?**
* **What are they allowed to do?**

---

## 👥 IAM Users

An **IAM User** represents an identity that can interact with AWS.

An IAM user can have:

* AWS Management Console access
* Programmatic access
* Permissions through policies
* Membership in IAM groups

Instead of using the AWS root account for everyday activities, individual identities should be created with only the permissions they require.

---

## 👨‍👩‍👧‍👦 IAM Groups

An **IAM Group** is a collection of IAM users.

Instead of assigning the same permissions individually to multiple users, permissions can be attached to a group.

Example:

```text
Developers
├── User-A
├── User-B
└── User-C
```

If the `Developers` group receives appropriate permissions, users belonging to the group receive those permissions.

### Important

IAM groups contain **users**, not other IAM groups.

---

# 📜 IAM Policies

IAM policies define permissions within AWS.

Policies specify which actions are:

* Allowed
* Denied

and the AWS resources to which those permissions apply.

IAM policies are written using **JSON**.

Example structure:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": "ec2:DescribeInstances",
      "Resource": "*"
    }
  ]
}
```

Important policy elements include:

| Element     | Purpose                                                 |
| ----------- | ------------------------------------------------------- |
| `Version`   | Policy language version                                 |
| `Statement` | Contains one or more permission statements              |
| `Effect`    | Specifies `Allow` or `Deny`                             |
| `Action`    | AWS API actions affected                                |
| `Resource`  | Resources to which the statement applies                |
| `Condition` | Optional conditions controlling when the policy applies |

---

# 🛡️ Multi-Factor Authentication (MFA)

**MFA** adds an additional authentication factor to an AWS account.

Instead of relying only on:

```text
Username + Password
```

MFA adds another factor:

```text
Username + Password
        +
       MFA
```

This provides additional protection if a password becomes compromised.

MFA is particularly important for highly privileged accounts.

---

# 🔑 AWS Access Keys

Access keys are credentials used for **programmatic access to AWS**.

They consist of:

```text
Access Key ID
      +
Secret Access Key
```

These credentials can be used by tools such as:

* AWS CLI
* AWS SDKs

> ⚠️ **Security Warning:** Access keys and secret access keys must never be uploaded to GitHub or exposed publicly.

---

# 💻 AWS CLI

The **AWS Command Line Interface (CLI)** allows AWS services and resources to be accessed and managed from a terminal.

Check the installed CLI version:

```bash
aws --version
```

The CLI can be configured with credentials and settings for an AWS environment.

Example:

```bash
aws configure
```

During configuration, information such as credentials, default Region and output format can be configured.

---

## CLI Example

A command such as:

```bash
aws iam list-users
```

can be used to request information about IAM users when the configured identity has the necessary permission.

This demonstrates an important IAM concept:

> **Using the CLI does not bypass IAM permissions.**

The identity making the request still needs authorization to perform the requested AWS action.

---

# 🧩 AWS SDK

**AWS Software Development Kits (SDKs)** allow applications to interact programmatically with AWS services.

SDKs are available for multiple programming languages.

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

---

# 🔐 Security Connection

IAM is especially important to my cybersecurity learning because it introduces core security concepts including:

### Authentication

Verifying **who an identity is**.

### Authorization

Determining **what an authenticated identity is allowed to do**.

### Principle of Least Privilege

Users and applications should receive only the permissions required to perform their tasks.

```text
Required Access
      ↓
Minimum Permissions
      ↓
Reduced Security Risk
```

### Credential Security

Credentials such as passwords and access keys must be protected.

Important practices include:

* Enable MFA
* Avoid using the root account for everyday activities
* Never expose secret access keys
* Never commit credentials to GitHub
* Avoid hard-coding credentials in applications
* Grant only required permissions
* Review permissions regularly

---

# 🧪 Hands-On Practice

During this section, I practiced:

* Creating IAM users
* Creating IAM groups
* Adding users to groups
* Working with IAM policies
* Testing permissions
* Enabling MFA
* Understanding AWS access keys
* Installing/configuring AWS CLI
* Interacting with AWS through CLI commands

Detailed hands-on documentation is maintained inside the [`labs/`](./labs/) directory.

---

# 🎯 Key Takeaways

* IAM controls access to AWS resources.
* IAM users represent individual identities.
* IAM groups simplify permission management for multiple users.
* IAM policies define permissions.
* MFA provides an additional layer of account protection.
* AWS CLI provides command-line access to AWS services.
* AWS SDKs allow applications to interact with AWS programmatically.
* AWS CLI requests are still controlled by IAM permissions.
* Access keys are sensitive credentials and must be protected.
* IAM permissions should follow the **Principle of Least Privilege**.
