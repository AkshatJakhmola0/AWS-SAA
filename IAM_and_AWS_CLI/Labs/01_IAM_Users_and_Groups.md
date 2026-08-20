# 👥 IAM Users & Groups — Hands-On Lab

## 🎯 Objective

Learn how to create and manage **IAM Users and Groups** and understand how permissions can be assigned through group membership.

---

## What I Practiced

* Creating an IAM user
* Creating an IAM group
* Adding users to groups
* Assigning permissions through groups
* Signing in as an IAM user
* Understanding the difference between the root account and IAM users

---

## IAM Users

An **IAM User** represents an identity inside an AWS account.

An IAM user can be created for a person who needs access to AWS resources.

```text
AWS Account
    │
    └── IAM
         │
         ├── User-A
         ├── User-B
         └── User-C
```

Each user can have their own credentials and permissions.

---

## IAM Groups

An **IAM Group** is a collection of IAM users.

Groups make permission management easier because permissions can be assigned to the group instead of individually to every user.

Example:

```text
Developers
├── User-A
├── User-B
└── User-C
```

If permissions are attached to the `Developers` group, members of that group receive those permissions.

> IAM groups contain users. Groups cannot contain other groups.

---

## 🧪 Hands-On Steps

### Step 1 — Open IAM

From the AWS Management Console:

```text
AWS Console
    ↓
IAM
    ↓
Users
```

### Step 2 — Create an IAM User

Created a new IAM user for hands-on practice.

### Step 3 — Create an IAM Group

Created an IAM group for managing permissions.

### Step 4 — Assign Permissions

Assigned the required permissions to the group.

### Step 5 — Add User to Group

Added the IAM user to the group.

```text
IAM User
    ↓
IAM Group
    ↓
Permissions
```

### Step 6 — Test Access

Signed in using the IAM user and verified the permissions available to that identity.

---

## 🔐 Security Connection

IAM users and groups demonstrate important cybersecurity concepts:

* Identity management
* Authentication
* Authorization
* Access control
* Least privilege

Instead of giving every user administrative access, permissions should be based on what the user actually needs.

---

## 🔒 Security Best Practices

* Avoid using the root account for everyday activities.
* Create separate identities for users.
* Use groups to simplify permission management.
* Grant only required permissions.
* Remove unnecessary access.
* Follow the **Principle of Least Privilege**.

---
