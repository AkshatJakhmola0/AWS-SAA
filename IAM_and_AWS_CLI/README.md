# 🔐 IAM & AWS CLI

This section documents my learning and hands-on practice with **AWS Identity and Access Management (IAM)**, **AWS CLI**, **AWS CloudShell**, IAM Roles, and IAM security tools while preparing for the **AWS Certified Solutions Architect – Associate (SAA-C03)**.

The main focus of this section is understanding how AWS manages **identities, authentication, authorization, permissions, and secure access to AWS resources**.

---

## 📚 Topics Covered

* IAM Introduction
* IAM Users
* IAM Groups
* IAM Policies
* IAM Permissions
* Multi-Factor Authentication (MFA)
* AWS Access Keys
* AWS CLI
* AWS SDK
* AWS CloudShell
* IAM Roles for AWS Services
* IAM Security Tools
* IAM Best Practices
* Principle of Least Privilege

---

# 🧠 What is AWS IAM?

**AWS Identity and Access Management (IAM)** is an AWS service used to securely control access to AWS resources.

IAM helps determine:

```text
WHO can access AWS?
        +
WHAT are they allowed to do?
```

These represent two fundamental security concepts:

* **Authentication** — verifying an identity
* **Authorization** — determining what that identity is permitted to do

---

# 👤 IAM Users

An **IAM User** represents an identity within an AWS account.

A user can be created for a person who requires access to AWS.

An IAM user can have:

* AWS Management Console access
* Permissions through IAM policies
* Membership in IAM groups
* Programmatic access when required

Example:

```text
AWS Account
    │
    └── IAM
         │
         ├── User-A
         ├── User-B
         └── User-C
```

Each person should use their own identity instead of sharing credentials.

---

# 👥 IAM Groups

An **IAM Group** is a collection of IAM users.

Groups simplify permission management when multiple users require similar permissions.

Example:

```text
Developers
│
├── User-A
├── User-B
└── User-C
```

Instead of assigning the same permissions separately to each developer:

```text
IAM Policy
    ↓
Developers Group
    ↓
All Developers
```

Users in the group receive the permissions associated with that group.

> IAM groups contain users. Groups cannot contain other groups.

---

# 📜 IAM Policies

IAM policies define permissions in AWS.

They specify which actions are **allowed or denied** and which resources those permissions apply to.

IAM policies are written using **JSON**.

Example:

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

## Important Policy Elements

| Element     | Purpose                                                |
| ----------- | ------------------------------------------------------ |
| `Version`   | Policy language version                                |
| `Statement` | Contains permission statements                         |
| `Effect`    | Specifies `Allow` or `Deny`                            |
| `Action`    | Specifies AWS API actions                              |
| `Resource`  | Specifies affected AWS resources                       |
| `Condition` | Optional conditions controlling when permissions apply |

Conceptually:

```text
IAM Identity
     ↓
IAM Policy
     ↓
Permission Evaluation
     ↓
AWS Resource
```

---

# ⚖️ Principle of Least Privilege

One of the most important IAM security principles is **Least Privilege**.

An identity should receive only the permissions necessary to perform its required tasks.

Instead of:

```text
User
 ↓
AdministratorAccess
 ↓
Everything
```

prefer:

```text
User
 ↓
Required Permissions
 ↓
Required Resources
```

Reducing unnecessary permissions reduces the potential impact of compromised credentials, mistakes, or unauthorized activity.

---

# 🔐 Multi-Factor Authentication (MFA)

**Multi-Factor Authentication (MFA)** adds another authentication factor in addition to a password.

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

MFA provides additional protection if a password becomes compromised.

It is especially important for highly privileged identities and the AWS root account.

---

# 👑 AWS Root Account

The **root user** is created when an AWS account is created and has extremely powerful access to the account.

The root account should therefore be strongly protected.

Important practices include:

```text
AWS Root Account
       │
       ├── Enable MFA
       ├── Protect credentials
       ├── Avoid everyday use
       └── Avoid unnecessary access keys
```

Administrative tasks should generally be performed using appropriately configured identities rather than routinely using the root account.

---

# 🔑 AWS Access Keys

Access keys can be used for **programmatic access** to AWS.

They consist of:

```text
Access Key ID
      +
Secret Access Key
```

Access keys can be used by tools such as:

* AWS CLI
* AWS SDKs
* Applications interacting with AWS APIs

Access keys are **sensitive credentials**.

> ⚠️ Access keys and secret access keys must never be exposed publicly or committed to GitHub.

---

# 💻 AWS CLI

The **AWS Command Line Interface (CLI)** allows AWS services to be accessed and managed through terminal commands.

Conceptually:

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

## Check AWS CLI Installation

```bash
aws --version
```

This verifies that AWS CLI is installed.

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

Actual credential values must never be included in public documentation or repositories.

---

## Example AWS CLI Command

```bash
aws iam list-users
```

This requests information about IAM users.

However, the command succeeds only if the identity making the request has permission to perform the corresponding AWS action.

---

# 🛡️ CLI and IAM Permissions

Using the AWS CLI does **not bypass IAM**.

Every AWS CLI request is still subject to authorization.

```text
CLI Command
     ↓
AWS API Request
     ↓
IAM Permission Evaluation
     ↓
Allowed / Denied
```

This is an important security concept because AWS access remains controlled regardless of whether resources are accessed through the console, CLI, SDK, or APIs.

---

# 🧩 AWS SDK

AWS provides **Software Development Kits (SDKs)** that allow applications to interact programmatically with AWS services.

SDKs are available for multiple programming languages.

Conceptually:

```text
Application
     ↓
AWS SDK
     ↓
AWS API
     ↓
AWS Services
```

Like CLI requests, SDK requests are also subject to AWS authentication and authorization.

---

# ☁️ AWS CloudShell

**AWS CloudShell** provides a browser-based shell that can be accessed from the AWS Management Console.

It provides a command-line environment for interacting with AWS without requiring a local CLI installation.

Conceptually:

```text
AWS Management Console
          ↓
      CloudShell
          ↓
       AWS CLI
          ↓
      AWS Services
```

CloudShell availability can depend on the AWS Region.

---

## Local AWS CLI vs CloudShell

| AWS CLI                      | AWS CloudShell                              |
| ---------------------------- | ------------------------------------------- |
| Runs on the local computer   | Runs in an AWS-managed browser shell        |
| Requires local installation  | No local installation required              |
| Requires local configuration | Integrated with the AWS console session     |
| Useful for local workflows   | Useful for quick browser-based AWS commands |

Both approaches remain subject to AWS IAM permissions.

---

# 🎭 IAM Roles

An **IAM Role** is an AWS identity with permissions that can be assumed by trusted entities.

Unlike a typical IAM user, a role is not intended to represent one permanent individual identity with long-term credentials.

Roles can be used by:

* AWS services
* Applications
* Users
* Other trusted identities

---

# ⚙️ IAM Roles for AWS Services

AWS services may need permission to interact with other AWS services.

IAM Roles provide a secure mechanism for granting those permissions.

Example:

```text
EC2 Instance
     ↓
IAM Role
     ↓
IAM Policy
     ↓
S3 Bucket
```

Instead of storing long-term AWS credentials directly on an EC2 instance, an appropriate IAM role can provide the required permissions.

This is a major security advantage.

---

# 🔑 Users vs Roles

| IAM User                         | IAM Role                             |
| -------------------------------- | ------------------------------------ |
| Represents an identity           | Assumable identity                   |
| Can have long-term credentials   | Commonly uses temporary credentials  |
| Often associated with a person   | Often used by AWS services/workloads |
| Has permissions through policies | Has permissions through policies     |

Choosing the appropriate identity type is an important part of secure AWS architecture.

---

# 🛡️ IAM Security Tools

AWS provides IAM-related tools that help review permissions and credentials.

Two important tools introduced in this section are:

## IAM Credentials Report

The **Credentials Report** provides account-level information about IAM users and the status of their credentials.

It can help review information related to:

* Password usage
* MFA
* Access keys
* Credential status

This is useful for security reviews and identifying credentials that may require attention.

---

## IAM Access Advisor

**Access Advisor** provides information about service permissions granted to an identity and when those services were last accessed.

This information can help identify permissions that may no longer be required.

Conceptually:

```text
Current Permissions
        ↓
Access Advisor
        ↓
Review Service Usage
        ↓
Remove Unnecessary Permissions
        ↓
Least Privilege
```

---

# 🔐 IAM Security Best Practices

Important IAM security practices include:

* Do not routinely use the root account.
* Protect the root account with MFA.
* Use individual identities rather than sharing credentials.
* Use IAM groups where appropriate.
* Follow the Principle of Least Privilege.
* Review permissions regularly.
* Remove unnecessary permissions.
* Protect passwords and access keys.
* Never publish AWS credentials.
* Avoid hard-coding credentials into applications.
* Prefer IAM roles for AWS services where appropriate.
* Remove credentials that are no longer required.
* Use IAM security tools to review credentials and permissions.

---

# 🔒 Cybersecurity Connection

IAM directly connects AWS architecture with several fundamental cybersecurity concepts.

## Authentication

Verifying:

> **Who are you?**

Examples include passwords and MFA.

---

## Authorization

Determining:

> **What are you allowed to do?**

IAM policies are a major mechanism for controlling authorization.

---

## Access Control

```text
Identity
    ↓
Authentication
    ↓
IAM Policies / Roles
    ↓
Authorization
    ↓
AWS Resource
```

---

## Credential Management

Passwords, access keys, and other credentials must be securely managed.

Compromised credentials can potentially allow unauthorized access according to the permissions associated with the identity.

---

## Least Privilege

Limiting permissions reduces the potential impact of:

* Credential compromise
* Misconfiguration
* Human error
* Unauthorized activity
* Excessive privileges

---

## Defense in Depth

AWS identity security should not depend on a single control.

For example:

```text
Strong Authentication
        +
       MFA
        +
Least Privilege
        +
IAM Roles
        +
Credential Reviews
        +
Monitoring
```

Together these controls provide stronger protection than relying on passwords alone.

---

# 🧪 Hands-On Labs

Hands-on practice for this section is documented separately inside the [`labs/`](./labs/) directory.

```text
labs/
│
├── 01_IAM_Users_and_Groups.md
├── 02_IAM_Policies.md
├── 03_IAM_MFA.md
├── 04_AWS_CLI.md
├── 05_AWS_CloudShell.md
├── 06_IAM_Roles.md
└── 07_IAM_Security_Tools.md
```

The labs document the practical work performed while learning each IAM concept.

---

# 🎯 Key Takeaways

* IAM controls authentication and authorization within AWS.
* IAM users represent identities within an AWS account.
* IAM groups simplify permission management for multiple users.
* IAM policies define what actions identities are authorized to perform.
* Least privilege is a fundamental AWS security principle.
* MFA provides an additional authentication factor.
* The root account should be strongly protected and not used for routine tasks.
* Access keys provide programmatic access and must be treated as sensitive credentials.
* AWS CLI allows AWS services to be managed through terminal commands.
* AWS SDKs allow applications to interact programmatically with AWS.
* AWS CloudShell provides browser-based command-line access to AWS.
* IAM roles allow trusted entities and AWS services to obtain required permissions without relying on embedded long-term credentials.
* IAM security tools help review credentials, access, and unnecessary permissions.
* AWS Console, CLI, SDK, and CloudShell access are all ultimately controlled through AWS identity and authorization mechanisms.

---
