# 🛡️ IAM Security Tools — Hands-On Lab

## 🎯 Objective

Understand and practice using **AWS IAM Security Tools** to review account credentials, analyze permissions, and support the **Principle of Least Privilege**.

The two IAM security tools covered in this section are:

* **IAM Credentials Report**
* **IAM Access Advisor**

---

# 🔐 Why IAM Security Tools Matter

Creating users, groups, policies, and roles is only one part of IAM security.

AWS environments also need to be reviewed to answer questions such as:

```text
Are users using MFA?
        ↓
Are access keys active?
        ↓
When were credentials last used?
        ↓
Are permissions actually being used?
        ↓
Can unnecessary permissions be removed?
```

IAM security tools help provide information that can support these reviews.

---

# 📋 IAM Credentials Report

The **IAM Credentials Report** is an account-level report containing information about IAM users and the status of their credentials.

It can help administrators review the security state of IAM users.

---

## Information Available

The Credentials Report can provide information related to:

* User creation
* Password status
* Password usage
* Password changes
* MFA status
* Access keys
* Access key status
* Access key usage

This makes the report useful for security auditing and credential reviews.

---

## Example Security Review

Conceptually:

```text
AWS Account
     ↓
IAM Credentials Report
     ↓
Review IAM Users
     │
     ├── Password Enabled?
     ├── MFA Enabled?
     ├── Access Keys Active?
     └── Credentials Recently Used?
```

This information can help identify accounts that may require further investigation or remediation.

---

# 🧪 Credentials Report Hands-On

## Step 1 — Open IAM

From the AWS Management Console:

```text
AWS Console
    ↓
IAM
    ↓
Credential Report
```

---

## Step 2 — Generate the Report

Generated the IAM Credentials Report for the AWS account.

---

## Step 3 — Download the Report

Downloaded the generated report for review.

---

## Step 4 — Review IAM Credential Information

Reviewed information associated with IAM users and their credentials.

The report can help identify situations such as:

* MFA not enabled
* Active access keys
* Old or unused credentials
* Password-related information

---

# 🔍 IAM Access Advisor

**IAM Access Advisor** provides information about AWS service permissions granted to an identity and when those services were last accessed.

This can help determine whether permissions are actually being used.

---

## Why Access Advisor is Useful

Consider an identity with permissions for:

```text
EC2
S3
RDS
Lambda
DynamoDB
```

Suppose the identity actually uses only:

```text
EC2
S3
```

Access information can help administrators investigate whether the remaining permissions are still necessary.

Conceptually:

```text
Granted Permissions
        ↓
   Access Advisor
        ↓
Review Service Usage
        ↓
Identify Unused Access
        ↓
Review Permissions
        ↓
Least Privilege
```

---

# 🧪 Access Advisor Hands-On

## Step 1 — Select an IAM Identity

Opened an IAM user, group, or role where access information was available.

---

## Step 2 — Review Access Information

Reviewed service access information associated with the identity.

---

## Step 3 — Identify Potentially Unused Permissions

Examined whether permissions granted to the identity appeared necessary based on service access information.

---

## Step 4 — Apply Least-Privilege Thinking

Used the information to understand how unnecessary permissions could potentially be identified and removed after proper review.

> Access information should assist a security review rather than automatically determine that a permission is safe to remove.

---

# ⚖️ Credentials Report vs Access Advisor

| Credentials Report               | Access Advisor                                             |
| -------------------------------- | ---------------------------------------------------------- |
| Account-level report             | Helps analyze service access                               |
| Focuses on IAM user credentials  | Focuses on granted service permissions and access activity |
| Useful for credential auditing   | Useful for permission reviews                              |
| Can show MFA-related information | Can help identify potentially unused permissions           |
| Can show access-key information  | Supports least-privilege decisions                         |

The tools therefore address different security questions.

```text
Credentials Report
       ↓
"How are IAM user credentials configured?"

Access Advisor
       ↓
"Which granted service permissions are being used?"
```

---

# 🔐 Cybersecurity Connection

These IAM tools connect directly with several cybersecurity practices.

## 1. Identity Auditing

Organizations need visibility into identities and credentials.

The Credentials Report can support reviews of IAM user credential security.

---

## 2. Credential Management

Unused or unnecessary credentials increase the attack surface.

For example:

```text
Old Access Key
      ↓
Forgotten Credential
      ↓
Potential Exposure
      ↓
Unauthorized Access Risk
```

Regular credential reviews help reduce this risk.

---

## 3. Least Privilege

Access Advisor can provide information useful when reviewing whether an identity has more permissions than it needs.

```text
Current Permissions
       ↓
Review Actual Usage
       ↓
Identify Unnecessary Access
       ↓
Reduce Permissions
       ↓
Smaller Attack Surface
```

---

## 4. Security Auditing

IAM security should not be treated as:

```text
Configure Once
     ↓
Forget
```

A stronger approach is:

```text
Configure
    ↓
Monitor
    ↓
Review
    ↓
Remove Unnecessary Access
    ↓
Repeat
```

---

# 🚨 Example Security Scenario

Imagine an IAM user has:

* Console access
* No MFA
* An active access key
* Broad AWS permissions

A security review may identify several areas requiring attention.

```text
IAM User
   │
   ├── MFA ❌
   ├── Access Key ✅
   └── Broad Permissions ⚠️
            ↓
      Security Review
            ↓
      Reduce Risk
```

The Credentials Report and Access Advisor can provide useful information during this type of review.

---

# 🔒 Security Best Practices

* Regularly review IAM users and credentials.
* Enable MFA where appropriate, especially for privileged access.
* Review active access keys.
* Remove credentials that are no longer required.
* Investigate old or unused credentials.
* Review permissions periodically.
* Remove unnecessary permissions after validating they are not required.
* Follow the **Principle of Least Privilege**.
* Avoid leaving unused IAM identities active indefinitely.
* Treat IAM security as an ongoing process.

---

# 💼 Job-Relevant Connection

IAM security tools introduce concepts that are useful for **cloud security, IAM, SOC, and security analyst roles**.

Relevant skills include:

* Identity auditing
* Access reviews
* Credential management
* Permission analysis
* Least privilege
* Security posture assessment
* Security reporting
* Cloud security monitoring

A security analyst should be able to think beyond:

> **"Does this user have access?"**

and also ask:

> **"Does this user still need this access?"**

---

# 🧠 Security Analyst Mindset

When reviewing an AWS identity, useful questions include:

```text
Who is this identity?
        ↓
How does it authenticate?
        ↓
Is MFA enabled?
        ↓
Does it have access keys?
        ↓
What permissions does it have?
        ↓
Which services does it actually use?
        ↓
Are all these permissions necessary?
```

This turns IAM configuration into an actual **security review process**.


# 🎯 Key Takeaways

* IAM security requires continuous review, not just initial configuration.
* The **Credentials Report** provides account-level information about IAM users and their credentials.
* Credentials Reports can assist with reviewing MFA, passwords, and access keys.
* **Access Advisor** provides information about granted service permissions and service access activity.
* Access information can help identify permissions that may require review.
* Unused credentials and excessive permissions increase security risk.
* IAM security tools support the **Principle of Least Privilege**.
* Credential and permission reviews are important parts of cloud security.
