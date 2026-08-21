# ☁️ AWS CloudShell — Hands-On Lab

## 🎯 Objective

Understand **AWS CloudShell**, its regional availability, and how it provides browser-based command-line access to AWS without requiring a local CLI installation.

---

## What is AWS CloudShell?

**AWS CloudShell** is a browser-based shell that is available directly from the **AWS Management Console**.

It provides a command-line environment for interacting with AWS services.

Conceptually:

```text
AWS Management Console
          ↓
      AWS CloudShell
          ↓
       AWS CLI
          ↓
      AWS Services
```

---

## Why CloudShell is Useful

CloudShell allows AWS CLI commands to be run directly from the browser.

This means a user does not always need to:

* Install AWS CLI locally
* Configure a separate terminal environment
* Manually set up CLI access on another computer

It is useful for quick AWS administration and learning activities.

---

## 🌍 Region Availability

AWS CloudShell is not necessarily available in every AWS Region.

Before using CloudShell, it is important to check whether the selected Region supports it.

The AWS Console Region selector can affect CloudShell availability.

```text
AWS Region
    ↓
CloudShell Supported?
    ↓
Yes → CloudShell Available
No  → Select Supported Region
```

---

## 🧪 Hands-On Steps

### Step 1 — Sign in to AWS Console

Logged in to the AWS Management Console using an IAM identity.

---

### Step 2 — Select a Supported Region

Verified that CloudShell was available in the selected AWS Region.

---

### Step 3 — Open AWS CloudShell

Opened CloudShell from the AWS Management Console.

CloudShell launched a terminal environment directly inside the browser.

---

### Step 4 — Verify AWS CLI

AWS CLI is available inside CloudShell.

A command such as:

```bash
aws --version
```

can be used to check the installed AWS CLI version.

---

### Step 5 — Run AWS CLI Commands

AWS commands can be executed directly from CloudShell.

Example:

```bash
aws iam list-users
```

The command is still subject to the permissions of the AWS identity using CloudShell.

---

# 🔐 CloudShell and IAM

Using CloudShell does **not bypass IAM permissions**.

The request flow can be understood as:

```text
AWS Identity
     ↓
CloudShell
     ↓
AWS CLI Command
     ↓
AWS API Request
     ↓
IAM Authorization
     ↓
Allowed / Denied
```

If the identity does not have permission to perform an AWS action, running that command through CloudShell will not grant additional access.

---

# 💻 Local AWS CLI vs AWS CloudShell

| Local AWS CLI                       | AWS CloudShell                              |
| ----------------------------------- | ------------------------------------------- |
| Runs on the user's computer         | Runs in an AWS-managed browser environment  |
| Requires local installation         | No local installation required              |
| Requires local setup/configuration  | Integrated with the AWS Console session     |
| Useful for regular local workflows  | Useful for quick browser-based AWS tasks    |
| Can be used outside the AWS Console | Accessed through the AWS Management Console |

Both methods ultimately interact with AWS APIs and remain subject to IAM authorization.

---

# 🔐 Security Connection

CloudShell connects to several important cybersecurity concepts.

## Authentication

The user must first authenticate to AWS before using CloudShell.

---

## Authorization

The actions that can be performed from CloudShell depend on the permissions assigned to the AWS identity.

---

## Credential Management

One advantage of CloudShell is that it can reduce the need to manually enter long-term access keys into a local CLI environment for simple console-based tasks.

However, normal AWS security practices still apply.

---

## Least Privilege

CloudShell should only be able to perform actions that the current AWS identity actually requires.

```text
User
 ↓
Required IAM Permissions
 ↓
CloudShell
 ↓
Allowed AWS Actions
```

---

# 🔒 Security Best Practices

* Use an IAM identity rather than the root account for normal CloudShell activities.
* Follow the Principle of Least Privilege.
* Do not expose sensitive data in terminal output.
* Do not paste access keys or secrets into scripts unnecessarily.
* Verify the selected AWS Region.
* Be careful when running commands that create, modify, or delete AWS resources.
* Review commands before executing them.

---


# 🎯 Key Takeaways

* AWS CloudShell provides a browser-based command-line environment.
* AWS CLI is available inside CloudShell.
* CloudShell availability depends on the AWS Region.
* CloudShell does not bypass IAM.
* AWS commands remain subject to the current identity's permissions.
* CloudShell is useful for quick AWS administration without installing AWS CLI locally.
* Least privilege and secure credential practices still apply.
