# IAM Security Notes 🔐

## Principle of Least Privilege

Grant only the permissions required to perform a task.

Avoid giving AdministratorAccess unless it is actually required.

## Root Account

The AWS root account has unrestricted access to the AWS account.

Security practices:

- Enable MFA
- Do not use root for everyday activities
- Do not create root access keys
- Protect root credentials

## MFA

MFA adds another authentication factor in addition to the password.

This reduces the risk associated with compromised passwords.

## Access Keys

Access keys must be treated as secrets.

Never:

- Commit access keys to GitHub
- Hard-code keys in applications
- Share secret access keys
- Store credentials in public repositories

## IAM Policies

Policies should follow least privilege.

Permissions should be restricted to:

- Required actions
- Required resources
- Required conditions where appropriate

## Cybersecurity Connection

IAM relates directly to:

- Identity and Access Management
- Authentication
- Authorization
- Least Privilege
- Credential Management
- Access Control
