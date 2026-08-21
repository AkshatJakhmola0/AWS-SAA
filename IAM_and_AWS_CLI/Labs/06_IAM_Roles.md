# 🎭 IAM Roles — Hands-On Lab

## 🎯 Objective

Understand **AWS IAM Roles**, how they differ from IAM users, and how roles allow AWS services to securely access other AWS resources without embedding long-term credentials.

---

## What is an IAM Role?

An **IAM Role** is an AWS identity that can be assigned permissions through IAM policies.

Unlike an IAM user, a role is designed to be **assumed by a trusted entity** when permissions are required.

Trusted entities can include:

* AWS services
* Applications
* IAM users
* Other AWS accounts

Conceptually:

```text
Trusted Entity
      ↓
Assumes IAM Role
      ↓
Receives Permissions
      ↓
Accesses AWS Resource
```

---

# 👤 IAM User vs IAM Role

An IAM user and an IAM role are both AWS identities, but they are designed for different purposes.

| IAM User                                | IAM Role                                |
| --------------------------------------- | --------------------------------------- |
| Usually represents a person or workload | Designed to be assumed                  |
| Can have long-term credentials          | Commonly provides temporary credentials |
| Can have permissions through policies   | Has permissions through policies        |
| Direct identity                         | Assumable identity                      |

---

# ⚙️ IAM Roles for AWS Services

AWS services sometimes need permission to interact with other AWS services.

For example, an **EC2 instance** may need permission to access an **S3 bucket**.

Instead of storing an Access Key ID and Secret Access Key directly on the EC2 instance, an IAM role can be used.

```text
EC2 Instance
     ↓
IAM Role
     ↓
IAM Policy
     ↓
S3 Bucket
```

The role provides the permissions required by the EC2 instance.

---

# 🔑 Why Use IAM Roles?

Consider an application running on EC2 that needs access to AWS resources.

A less secure approach would be:

```text
EC2 Application
      ↓
Hard-Coded Access Keys
      ↓
AWS Service
```

This creates credential-management risks.

A better approach is:

```text
EC2 Application
      ↓
IAM Role
      ↓
Temporary Credentials
      ↓
AWS Service
```

This reduces dependence on long-term credentials stored inside applications or servers.

---

# 🧪 Hands-On Practice

## Step 1 — Open IAM

From the AWS Management Console:

```text
AWS Console
    ↓
IAM
    ↓
Roles
```

---

## Step 2 — Create a Role

Started the process of creating a new IAM role.

---

## Step 3 — Select Trusted Entity

Selected the entity/service that should be allowed to use the role.

For an AWS service role, this can be represented as:

```text
AWS Service
     ↓
Trusted Entity
     ↓
IAM Role
```

---

## Step 4 — Assign Permissions

Attached the appropriate permissions to the role.

```text
IAM Role
    ↓
IAM Policy
    ↓
Allowed AWS Actions
```

The permissions associated with the role determine what actions can be performed after the role is assumed.

---

## Step 5 — Review the Role

Reviewed the role configuration, including:

* Trusted entity
* Attached permissions
* Role information

---

# 🛡️ Trust and Permissions

There are two important questions when working with IAM roles:

### 1. Who can assume the role?

The role must trust an appropriate entity.

```text
Trusted Entity
      ↓
IAM Role
```

### 2. What can the role do?

Policies associated with the role determine its permissions.

```text
IAM Role
    ↓
Permissions
    ↓
AWS Resources
```

Conceptually:

```text
WHO CAN USE IT?
       ↓
Trusted Entity
       ↓
    IAM Role
       ↓
IAM Permissions
       ↓
WHAT CAN IT DO?
```

---

# 🔐 Cybersecurity Connection

IAM Roles are highly relevant to cloud security because they help manage access without unnecessarily distributing long-term credentials.

## Credential Security

Hard-coded credentials can create security risks.

For example:

```text
Application
    ↓
Access Key Stored in Code
    ↓
Code Uploaded to GitHub
    ↓
Credential Exposure
```

Using an appropriate IAM role can avoid this pattern for supported AWS workloads.

---

## Temporary Credentials

Roles commonly use **temporary security credentials** when they are assumed.

This is preferable to unnecessarily maintaining long-term credentials for workloads.

---

## Principle of Least Privilege

Roles should receive only the permissions they actually require.

Instead of:

```text
EC2
 ↓
IAM Role
 ↓
AdministratorAccess
```

prefer:

```text
EC2
 ↓
IAM Role
 ↓
Required Permission
 ↓
Required AWS Resource
```

---

# 🔒 Security Best Practices

* Prefer IAM roles for AWS services where appropriate.
* Avoid embedding long-term AWS credentials in applications.
* Follow the Principle of Least Privilege.
* Grant roles only the permissions they require.
* Configure trusted entities carefully.
* Review role permissions regularly.
* Remove unused roles and unnecessary permissions.
* Avoid giving roles broad administrative access without a genuine requirement.

---

# 💼 Job-Relevant Connection

Understanding IAM roles is useful beyond the SAA-C03 exam.

It connects directly with:

* Identity and Access Management
* Cloud Security
* Access Control
* Authentication and Authorization
* Least Privilege
* Credential Management
* Secure Cloud Architecture

For a cloud/security role, it is important to understand not only **how to create a role**, but also:

> **Who can assume the role, what permissions the role provides, and why those permissions are required.**

---


# 🎯 Key Takeaways

* IAM Roles are AWS identities that can be assumed by trusted entities.
* Roles can be used by AWS services, applications, users, and other trusted identities.
* Permissions are assigned to roles using IAM policies.
* AWS services can use roles to access other AWS resources.
* Roles commonly provide temporary credentials.
* Roles help reduce the need for hard-coded long-term credentials.
* IAM roles should follow the **Principle of Least Privilege**.
* Both the role's trust relationship and its permissions are important security considerations.
