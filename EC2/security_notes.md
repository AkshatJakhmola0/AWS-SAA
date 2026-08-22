# 🔐 Amazon EC2 — Security Notes

These notes focus specifically on the **security concepts and cybersecurity connections** covered while learning Amazon EC2 for the AWS Certified Solutions Architect – Associate (SAA-C03).

---

# 🛡️ EC2 Security Overview

Running an EC2 instance means operating a virtual server in AWS.

Securing the instance involves multiple layers:

```text
Internet
   ↓
Network Security
   ↓
Security Groups
   ↓
EC2 Instance
   ↓
Operating System
   ↓
Application
   ↓
IAM Permissions
   ↓
AWS Resources
```

Security should therefore not depend on a single control.

---

# 🌐 1. Network Exposure

An EC2 instance may expose services to a network or the internet.

Examples:

| Service |   Port | Purpose                       |
| ------- | -----: | ----------------------------- |
| SSH     |   `22` | Linux remote administration   |
| HTTP    |   `80` | Web traffic                   |
| HTTPS   |  `443` | Encrypted web traffic         |
| RDP     | `3389` | Windows remote administration |

Every unnecessary exposed service can increase the **attack surface**.

```text
More Open Ports
      ↓
More Exposed Services
      ↓
Larger Attack Surface
```

### Security Principle

> Only expose ports and services that are actually required.

---

# 🧱 2. Security Groups

AWS Security Groups act as **virtual firewalls** for resources such as EC2 instances.

They control:

* Inbound traffic
* Outbound traffic

A Security Group rule can consider:

```text
Protocol
   +
Port
   +
Source / Destination
```

---

## Inbound Rules

Inbound rules determine which traffic is allowed to reach the associated resource.

Example web server:

```text
Internet
   │
   │ HTTP :80
   ▼
Security Group
   │
   ▼
EC2 Web Server
```

---

## Restrict Administrative Access

SSH is an administrative service.

A broad rule such as:

```text
SSH
Port: 22
Source: 0.0.0.0/0
```

allows connection attempts from any IPv4 address.

Where appropriate, administrative access should instead be restricted to a trusted source.

```text
Trusted Administrator IP
          ↓
       SSH :22
          ↓
   Security Group
          ↓
     EC2 Instance
```

---

# 🔄 3. Security Groups Are Stateful

Security Groups are **stateful**.

When permitted traffic enters through a Security Group, response traffic for that connection is automatically allowed.

Conceptually:

```text
Client
  │
  │ Allowed Request
  ▼
Security Group
  │
  ▼
EC2
  │
  │ Response
  ▼
Client
```

Understanding stateful filtering is important when troubleshooting AWS network connectivity.

---

# 🔑 4. SSH Security

**SSH (Secure Shell)** is commonly used for remote administration of Linux EC2 instances.

Default SSH port:

```text
TCP 22
```

SSH provides powerful administrative access, so it should be carefully protected.

Important considerations include:

* Restricting network access
* Protecting private keys
* Using the correct identity
* Avoiding unnecessary internet exposure

---

# 🗝️ 5. EC2 Key Pair Security

SSH commonly uses public-key authentication.

```text
Public Key
     ↓
EC2 Instance

Private Key
     ↓
Administrator
```

The **private key must remain private**.

If an attacker obtains a usable private key and can reach the SSH service, unauthorized access may become possible depending on the environment.

---

## 🚨 Never Commit Private Keys

Files such as:

```text
my-key.pem
private.key
server-key.pem
```

must never be committed to a public GitHub repository.

For my AWS repositories, sensitive key files should be excluded.

Example `.gitignore`:

```gitignore
*.pem
*.key
.env
credentials
```

---

# 💻 6. SSH Troubleshooting From a Security Perspective

When SSH fails, check:

```text
Is EC2 running?
       ↓
Does it have network connectivity?
       ↓
Is TCP 22 permitted?
       ↓
Is the source IP permitted?
       ↓
Is the correct username being used?
       ↓
Is the correct private key being used?
```

This is useful cybersecurity practice because it combines:

* Networking
* Ports
* Firewall rules
* Authentication
* Linux
* Troubleshooting

---

# 🌐 7. EC2 Instance Connect

EC2 Instance Connect provides another method for connecting to supported EC2 instances.

Conceptually:

```text
AWS Environment
      ↓
EC2 Instance Connect
      ↓
EC2 Instance
```

Using another connection method does not mean security controls disappear.

Relevant IAM and network requirements still apply.

---

# 🎭 8. IAM Roles for EC2

Applications running on EC2 may need access to other AWS services.

For example:

```text
EC2
 ↓
S3
```

One approach would be to store long-term credentials on the server:

```text
Application
    ↓
Access Key
    ↓
Secret Key
    ↓
AWS API
```

This introduces credential-management risk.

A better AWS approach for supported workloads is to use an **IAM Role**.

```text
EC2
 ↓
IAM Role
 ↓
Temporary Credentials
 ↓
AWS API
 ↓
Required AWS Resource
```

---

# 🔐 9. Why IAM Roles Improve Security

IAM Roles can reduce reliance on long-term credentials stored on EC2 instances.

This helps protect against:

* Hard-coded credential exposure
* Credentials accidentally pushed to GitHub
* Long-lived credentials remaining on servers
* Manual credential distribution

However, a role itself must still be configured securely.

---

# ⚖️ 10. Least Privilege for EC2 Roles

Giving an EC2 instance a role does not automatically make it secure.

The role should contain only the permissions required by the workload.

Avoid:

```text
EC2
 ↓
AdministratorAccess
 ↓
Entire AWS Account
```

Prefer:

```text
EC2
 ↓
IAM Role
 ↓
Required Action
 ↓
Required Resource
```

This follows the **Principle of Least Privilege**.

---

# 🔓 11. Public vs Private Exposure

An EC2 instance that is reachable from the internet has a different risk profile from an internal-only workload.

Conceptually:

```text
Public EC2
   ↓
Internet Reachable
   ↓
Greater External Exposure
```

versus:

```text
Private Workload
   ↓
Restricted Network Access
   ↓
Reduced Direct Exposure
```

Public exposure is not automatically insecure, but it should exist only when required and should be protected appropriately.

---

# 🌍 12. Public IP Security

A public IP can make an appropriately routed EC2 instance reachable from the internet, subject to network controls.

Security therefore depends on more than simply whether a public IP exists.

```text
Internet
   ↓
Public IP
   ↓
Network Configuration
   ↓
Security Group
   ↓
EC2 Service
```

The exposed service and permitted traffic are what need to be carefully controlled.

---

# 🌐 13. HTTP vs HTTPS

HTTP commonly uses:

```text
TCP 80
```

HTTPS commonly uses:

```text
TCP 443
```

HTTP traffic itself is not encrypted by TLS.

HTTPS uses TLS to protect web traffic in transit.

Conceptually:

```text
HTTP
Client ────────────── Server
       Unencrypted

HTTPS
Client ══════════════ Server
       TLS Protected
```

For sensitive production web traffic, encrypted communication should be preferred.

---

# 🧠 14. Attack Surface

An **attack surface** consists of the potential points through which a system may be attacked.

For an EC2 server, this can include:

```text
EC2 Attack Surface
│
├── Publicly exposed services
├── Open ports
├── SSH access
├── Application vulnerabilities
├── Operating-system vulnerabilities
├── Weak credentials
├── Excessive IAM permissions
└── Misconfiguration
```

Reducing unnecessary exposure helps reduce the attack surface.

---

# 🧅 15. Defense in Depth

EC2 security should use multiple layers.

```text
                Internet
                   ↓
            Network Controls
                   ↓
            Security Groups
                   ↓
             EC2 Instance
                   ↓
              OS Security
                   ↓
         Application Security
                   ↓
              IAM Roles
                   ↓
          AWS Resource Access
```

Possible controls include:

* Restricted network access
* Security Groups
* Secure SSH configuration
* Protected private keys
* IAM least privilege
* Operating-system security
* Application security
* Logging and monitoring

If one control fails, other controls can still reduce risk.

---

# 👤 16. Authentication vs Authorization

EC2 provides another good example of the difference between these concepts.

### Authentication

> Who are you?

Examples:

* SSH key authentication
* AWS identity authentication

### Authorization

> What are you allowed to do?

Examples:

* IAM policies
* IAM roles
* OS permissions

An authenticated identity should still receive only the authorization it requires.

---

# 🔄 17. EC2 + IAM Security Flow

Consider an application running on EC2 that needs to access S3:

```text
User
 ↓
Connects to EC2
 ↓
Application
 ↓
EC2 IAM Role
 ↓
IAM Permission Evaluation
 ↓
S3
```

Different security controls operate at different points in this architecture.

This demonstrates why **cloud security involves identity, networking, systems, and applications together**.

---

# 💰 18. Security and Cost Awareness

Security labs can accidentally create costs if resources remain running.

After hands-on exercises, resources should be reviewed and unnecessary resources terminated or removed.

AWS Budgets can provide additional cost visibility.

```text
AWS Resources
     ↓
Usage
     ↓
Cost
     ↓
AWS Budget
     ↓
Alert
```

Cost management is an important operational habit when working with cloud infrastructure.

---

# 🚨 Common EC2 Security Mistakes

Examples of configurations to avoid include:

### ❌ Exposing SSH unnecessarily

```text
TCP 22 → 0.0.0.0/0
```

### ❌ Uploading private keys to GitHub

```text
github.com/project/my-key.pem
```

### ❌ Hard-coding AWS credentials

```text
ACCESS_KEY = "..."
SECRET_KEY = "..."
```

### ❌ Giving EC2 excessive IAM permissions

```text
EC2 → AdministratorAccess
```

### ❌ Leaving unnecessary services exposed

```text
Open Ports
   ↓
Unused Services
   ↓
Unnecessary Attack Surface
```

### ❌ Forgetting unused resources

Unused cloud resources can create unnecessary cost and potentially leave unnecessary infrastructure active.

---

# 🛡️ Secure EC2 Mindset

When reviewing an EC2 instance from a security perspective, ask:

```text
Does this instance need to be public?
            ↓
Which ports need to be open?
            ↓
Who should be allowed to connect?
            ↓
How is SSH protected?
            ↓
Are private keys secure?
            ↓
What IAM role is attached?
            ↓
Does the role have excessive permissions?
            ↓
Which services are running?
            ↓
Are unnecessary services exposed?
```

These questions turn basic EC2 configuration into **security analysis**.

---

# 💼 Job-Relevant Security Skills

The EC2 section reinforces several skills relevant to cybersecurity and cloud-security roles:

* TCP/IP
* Network ports
* Firewall concepts
* Security Groups
* SSH
* Linux remote administration
* Public/private key authentication
* Credential management
* IAM
* IAM Roles
* Least privilege
* Network exposure
* Attack surface reduction
* Troubleshooting
* Defense in depth

---

# 🎯 Key Security Takeaways

* Security Groups act as stateful virtual firewalls for AWS resources.
* Only required network traffic should be permitted.
* SSH commonly uses TCP port `22`.
* Administrative access should be restricted whenever possible.
* EC2 private keys must be securely protected.
* Private keys must never be committed to GitHub.
* IAM Roles are preferable to embedding long-term AWS credentials in EC2 workloads.
* EC2 IAM Roles should follow least privilege.
* Publicly exposed services increase the attack surface.
* HTTP and HTTPS provide different levels of transport protection.
* Authentication and authorization are separate security concepts.
* Secure EC2 architecture requires multiple layers of protection.
* Cloud security combines networking, identity, operating-system, application, and monitoring controls.
* Security configuration should be continuously reviewed rather than treated as a one-time task.
