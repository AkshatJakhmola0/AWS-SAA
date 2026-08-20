# 📜 IAM Policies — Hands-On Lab

## 🎯 Objective

Understand how **IAM Policies** control authorization in AWS and practice assigning and testing permissions.

---

## What I Practiced

* Exploring IAM policies
* Attaching policies
* Understanding policy permissions
* Testing permissions
* Understanding `Allow` and `Deny`
* Reading the basic structure of IAM policy JSON

---

## What is an IAM Policy?

An IAM policy defines permissions for AWS identities and resources.

Policies determine which AWS actions are allowed or denied.

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

## IAM Policy Structure

IAM policies use **JSON**.

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

---

## Important Policy Elements

| Element     | Purpose                                                |
| ----------- | ------------------------------------------------------ |
| `Version`   | Policy language version                                |
| `Statement` | Contains one or more permission statements             |
| `Effect`    | Specifies `Allow` or `Deny`                            |
| `Action`    | Specifies AWS API actions                              |
| `Resource`  | Specifies the AWS resources affected                   |
| `Condition` | Optional conditions controlling when permissions apply |

---

## 🧪 Hands-On Steps

### Step 1 — Open IAM Policies

```text
AWS Console
    ↓
IAM
    ↓
Policies
```

### Step 2 — Explore Policies

Reviewed available IAM policies and the permissions provided by them.

### Step 3 — Attach a Policy

Attached appropriate permissions to an IAM identity/group during the hands-on exercise.

### Step 4 — Test Permissions

Used the IAM user to verify which AWS actions were allowed.

This demonstrated that access to AWS resources depends on the policies associated with the identity.

---

## Principle of Least Privilege

One of the most important security principles in IAM is **Least Privilege**.

An identity should receive only the permissions required to perform its task.

Instead of:

```text
User
 ↓
AdministratorAccess
 ↓
Access Everything
```

Prefer:

```text
User
 ↓
Required Permissions
 ↓
Required Resources
```

---

## 🔐 Security Connection

Improper IAM policies can result in excessive permissions.

This can increase the impact of:

* Compromised credentials
* Insider threats
* Human mistakes
* Unauthorized access
* Privilege misuse

IAM policies are therefore an important part of AWS access control.

---

## 🔒 Security Best Practices

* Follow least privilege.
* Avoid unnecessary administrative permissions.
* Review permissions regularly.
* Remove unused permissions.
* Restrict permissions to required resources where possible.
* Understand what a policy allows before attaching it.

---
