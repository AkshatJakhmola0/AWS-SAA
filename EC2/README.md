# 🖥️ Amazon EC2 — Elastic Compute Cloud

This section documents my learning and hands-on practice with **Amazon EC2 (Elastic Compute Cloud)** while preparing for the **AWS Certified Solutions Architect – Associate (SAA-C03)**.

This section covers EC2 fundamentals, instance types, User Data, Security Groups, SSH, EC2 Instance Connect, IAM Roles, and EC2 purchasing options.

---

## 📚 Topics Covered

* AWS Budget Setup
* Amazon EC2 Fundamentals
* Launching EC2 Instances
* EC2 User Data
* EC2 Instance Types
* Security Groups
* Classic Ports
* SSH
* SSH Troubleshooting
* EC2 Instance Connect
* IAM Roles for EC2
* EC2 Purchasing Options
* Spot Instances
* Spot Fleet
* EC2 Launch Types

---

# 💰 AWS Budget Setup

Before creating AWS resources, it is important to monitor cloud spending.

AWS Budgets can be used to:

* Define a budget
* Monitor AWS costs
* Track usage
* Configure alerts when defined thresholds are reached

Conceptually:

```text
AWS Usage
    ↓
AWS Billing
    ↓
AWS Budget
    ↓
Budget Threshold
    ↓
Alert
```

Cost awareness is especially important when performing hands-on AWS labs because some AWS resources can generate charges while running.

---

# 🖥️ What is Amazon EC2?

**Amazon Elastic Compute Cloud (EC2)** provides virtual computing capacity in AWS.

EC2 instances can be used to run:

* Web servers
* Applications
* Development environments
* Backend services
* Databases
* Various workloads

Instead of purchasing and maintaining physical servers, computing resources can be provisioned in AWS when required.

```text
Traditional Infrastructure

Buy Physical Server
        ↓
Install Hardware
        ↓
Configure Server
        ↓
Run Application
```

With EC2:

```text
AWS
 ↓
Launch EC2 Instance
 ↓
Configure Instance
 ↓
Run Application
```

---

# ⚙️ EC2 Instance Configuration

When launching an EC2 instance, several components can be configured.

These include:

```text
EC2 Instance
│
├── Amazon Machine Image (AMI)
├── Instance Type
├── Storage
├── Network
├── Security Group
├── Key Pair
├── IAM Role
└── User Data
```

Each component affects how the instance operates.

---

# 💿 Amazon Machine Image — AMI

An **Amazon Machine Image (AMI)** provides the information required to launch an EC2 instance.

An AMI can include:

* Operating system
* Software configuration
* Application environment

Conceptually:

```text
AMI
 ↓
EC2 Instance
 ↓
Running Server
```

Different AMIs can be selected depending on the required operating system and workload.

---

# 🚀 Launching an EC2 Instance

A basic EC2 launch process can be represented as:

```text
Select AMI
    ↓
Select Instance Type
    ↓
Configure Network
    ↓
Configure Storage
    ↓
Configure Security Group
    ↓
Select/Create Key Pair
    ↓
Launch Instance
```

After launching, the instance transitions toward a running state and can be accessed according to its network and security configuration.

---

# 📜 EC2 User Data

**EC2 User Data** can be used to provide commands or scripts that execute during the initial startup process of an instance.

This can automate tasks such as:

* Installing packages
* Installing a web server
* Creating files
* Starting services
* Performing initial configuration

Example concept:

```text
Launch EC2
    ↓
Execute User Data
    ↓
Install Web Server
    ↓
Create Web Page
    ↓
Start Service
    ↓
Website Available
```

A simple Linux User Data script might look like:

```bash
#!/bin/bash

yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd

echo "<h1>Hello from EC2</h1>" > /var/www/html/index.html
```

The exact commands depend on the operating system/AMI being used.

---

# 🌐 EC2 Web Server Architecture

The hands-on web-server exercise demonstrates how several AWS and networking concepts work together.

```text
Internet
   ↓
Public IP
   ↓
Security Group
   ↓
HTTP Port 80
   ↓
EC2 Instance
   ↓
Web Server
   ↓
Website
```

For users on the internet to reach the web server, the required network traffic must be permitted.

---

# 🧠 EC2 Instance Types

EC2 provides different instance types optimized for different workloads.

An instance type determines available resources such as:

* CPU
* Memory
* Networking capability
* Other performance characteristics

Different workloads require different combinations of resources.

---

## EC2 Instance Type Categories

Common categories include:

### General Purpose

Balanced compute, memory, and networking resources.

Useful for many common workloads.

---

### Compute Optimized

Designed for workloads requiring strong CPU performance.

Examples can include:

* Compute-intensive applications
* Batch processing
* High-performance web servers

---

### Memory Optimized

Designed for workloads requiring large amounts of memory.

Examples can include:

* In-memory databases
* Large datasets
* Memory-intensive applications

---

### Storage Optimized

Designed for workloads requiring high-performance local storage.

---

# 🛡️ Security Groups

A **Security Group** acts as a virtual firewall controlling traffic associated with AWS resources such as EC2 instances.

Security Groups contain:

* **Inbound rules**
* **Outbound rules**

```text
Internet
   ↓
Inbound Rules
   ↓
Security Group
   ↓
EC2 Instance
   ↓
Outbound Rules
   ↓
Destination
```

---

# 📥 Inbound Traffic

Inbound rules determine what network traffic is allowed **to reach** the resource.

A rule can consider information such as:

* Protocol
* Port
* Source

Example:

```text
Source
   ↓
TCP
   ↓
Port 22
   ↓
Security Group
   ↓
EC2
```

---

# 📤 Outbound Traffic

Outbound rules determine what traffic is allowed **from** the associated resource.

```text
EC2
 ↓
Security Group
 ↓
Outbound Rule
 ↓
Destination
```

---

# 🔄 Security Groups are Stateful

Security Groups are **stateful**.

If permitted traffic enters an EC2 instance, the corresponding response traffic is automatically allowed regardless of outbound-rule evaluation for that response.

This stateful behavior is an important concept when studying AWS networking.

---

# 🚪 Classic Ports

Understanding common ports is important for both AWS and cybersecurity.

| Service |   Port | Purpose                            |
| ------- | -----: | ---------------------------------- |
| SSH     |   `22` | Secure remote Linux administration |
| FTP     |   `21` | File transfer                      |
| HTTP    |   `80` | Unencrypted web traffic            |
| HTTPS   |  `443` | Encrypted web traffic              |
| RDP     | `3389` | Remote Windows administration      |

These port numbers become particularly important when configuring Security Groups.

---

# 🔐 Security Group Example

Consider a Linux EC2 web server.

It may require:

```text
HTTP
Port: 80
Source: Internet

SSH
Port: 22
Source: Administrator IP
```

Conceptually:

```text
                    Internet
                       │
                    HTTP :80
                       │
                       ▼
                Security Group
                       │
                       ▼
                  EC2 Web Server
                       ▲
                       │
                    SSH :22
                       │
                 Administrator
```

The goal should be to expose only the services that are actually required.

---

# 🔑 SSH

**Secure Shell (SSH)** is commonly used to securely connect to and administer Linux systems remotely.

AWS EC2 Linux instances can be accessed using SSH when properly configured.

```text
Local Computer
      ↓
     SSH
      ↓
Security Group
      ↓
EC2 Instance
```

SSH commonly uses:

```text
TCP Port 22
```

---

# 🔐 EC2 Key Pairs

SSH access to EC2 commonly uses public-key cryptography.

During EC2 configuration, a key pair can be selected or created.

Conceptually:

```text
Public Key
    ↓
Associated with EC2

Private Key
    ↓
Kept securely by user
```

The private key must be protected.

> ⚠️ **Private `.pem` or other key files must never be committed to GitHub.**

---

# 💻 SSH Example

A typical SSH connection can conceptually look like:

```bash
ssh -i my-key.pem ec2-user@PUBLIC_IP
```

The exact username depends on the AMI being used.

For security reasons, real IP addresses and private key information should not be unnecessarily published.

---

# 🛠️ SSH Troubleshooting

If an SSH connection fails, several areas may need to be checked.

```text
SSH Failure
    │
    ├── Is the EC2 instance running?
    │
    ├── Is Port 22 allowed?
    │
    ├── Is the source IP correct?
    │
    ├── Is the correct private key being used?
    │
    ├── Is the username correct?
    │
    └── Does the instance have network connectivity?
```

Troubleshooting requires checking both the EC2 configuration and the network/security configuration.

---

# 🌐 EC2 Instance Connect

**EC2 Instance Connect** provides another method for connecting to supported EC2 instances.

It can provide browser-based connectivity from the AWS environment for supported configurations.

Conceptually:

```text
AWS Console
     ↓
EC2 Instance Connect
     ↓
EC2 Instance
```

This can be useful when a local SSH client is not being used.

Network and IAM requirements still apply.

---

# 🎭 IAM Roles for EC2

EC2 instances sometimes need permission to interact with other AWS services.

For example:

```text
EC2
 ↓
Needs Access
 ↓
S3
```

One insecure approach would be to store long-term AWS credentials directly on the EC2 instance.

```text
EC2
 ↓
Hard-Coded Access Keys
 ↓
S3
```

A better AWS approach is to use an **IAM Role**.

```text
EC2 Instance
     ↓
IAM Role
     ↓
Temporary Credentials
     ↓
AWS Service
```

This allows the EC2 workload to obtain permissions without unnecessarily embedding long-term credentials.

---

# 🔐 EC2 IAM Role Security

IAM roles should follow the **Principle of Least Privilege**.

Instead of:

```text
EC2
 ↓
AdministratorAccess
 ↓
Everything
```

prefer:

```text
EC2
 ↓
IAM Role
 ↓
Required Permissions
 ↓
Required AWS Resource
```

This reduces the impact of a compromised EC2 workload.

---

# 💰 EC2 Purchasing Options

AWS provides different EC2 purchasing options depending on workload requirements.

Important options include:

* On-Demand Instances
* Reserved Instances
* Savings Plans
* Spot Instances
* Dedicated Hosts
* Dedicated Instances

---

# 🟢 On-Demand Instances

On-Demand instances provide flexibility without requiring a long-term commitment.

They are useful when workloads:

* Are short-term
* Are unpredictable
* Cannot commit to long-term usage

---

# 📅 Reserved Instances

Reserved Instances can provide pricing benefits when committing to a longer usage period for eligible EC2 usage.

They can be useful for workloads with predictable requirements.

---

# 💵 Savings Plans

Savings Plans provide discounted pricing in exchange for a commitment to a certain amount of compute usage over a period.

---

# ⚡ Spot Instances

**Spot Instances** allow use of spare EC2 capacity at potentially significant discounts.

However, AWS can interrupt Spot capacity when it is required elsewhere.

Therefore, Spot Instances are better suited for workloads that can tolerate interruption.

Examples include:

* Batch processing
* Distributed workloads
* Data analysis
* Flexible processing jobs

They are generally not suitable for workloads that cannot tolerate interruption without appropriate architecture.

---

# 🚀 Spot Fleet

A **Spot Fleet** can be used to request and manage a collection of Spot Instances and, depending on configuration, other capacity to meet defined capacity requirements.

Conceptually:

```text
Spot Fleet
    │
    ├── Instance Type A
    ├── Instance Type B
    └── Instance Type C
          ↓
     Target Capacity
```

Using multiple instance types/capacity options can help improve the ability to obtain the required compute capacity.

---

# 🏢 Dedicated Hosts

A Dedicated Host provides a physical server dedicated to a customer's use.

This can be relevant for:

* Compliance requirements
* Licensing requirements
* Workloads requiring dedicated physical infrastructure

---

# 🖥️ Dedicated Instances

Dedicated Instances run on hardware dedicated to a single customer account, while AWS manages the placement of those instances.

---

# 📊 Purchasing Option Overview

| Option              | Typical Use                                    |
| ------------------- | ---------------------------------------------- |
| On-Demand           | Flexible/unpredictable workloads               |
| Reserved Instances  | Predictable longer-term usage                  |
| Savings Plans       | Committed compute usage                        |
| Spot Instances      | Fault-tolerant/interruption-tolerant workloads |
| Dedicated Hosts     | Dedicated physical server requirements         |
| Dedicated Instances | Instances on dedicated hardware                |

Choosing the correct purchasing option depends on:

```text
Workload
   ↓
Duration
   +
Predictability
   +
Interruption Tolerance
   +
Compliance
   +
Cost
```

---

# 🔐 Cybersecurity Connection

EC2 introduces several concepts directly relevant to cybersecurity.

## Network Exposure

Every exposed service increases the potential attack surface.

For example:

```text
Internet
   ↓
Open Port
   ↓
Running Service
   ↓
Potential Attack Surface
```

Only required ports should be exposed.

---

## SSH Security

SSH provides administrative access to a server.

Port `22` should not be unnecessarily exposed to the entire internet.

Instead of:

```text
SSH :22
Source: 0.0.0.0/0
```

a more restrictive source should be used when appropriate, such as the administrator's trusted IP/network.

---

## Key Management

Private SSH keys are security credentials.

They must be:

* Protected
* Stored securely
* Kept out of GitHub
* Removed/replaced when no longer trusted

---

## IAM Roles

IAM Roles can reduce the need to store long-term AWS credentials on EC2 instances.

This supports better credential management.

---

## Least Privilege

Least privilege applies to both:

**Network access**

```text
Only Required Ports
```

and:

**AWS permissions**

```text
Only Required IAM Permissions
```

---

## Defense in Depth

A secure EC2 environment should not depend on one security control.

Conceptually:

```text
IAM
 +
Security Groups
 +
SSH Key Security
 +
OS Security
 +
Patching
 +
Logging/Monitoring
 +
Least Privilege
        ↓
Defense in Depth
```

---

# 💼 Job-Relevant Skills

This EC2 section connects with several skills that commonly matter in cloud and cybersecurity roles:

* TCP/IP
* Network ports
* HTTP/HTTPS
* SSH
* Linux administration
* Firewalls
* Access control
* Public/private key concepts
* Cloud infrastructure
* IAM
* Least privilege
* Troubleshooting
* Secure remote access

For my cybersecurity journey, **Security Groups, SSH, IAM Roles, networking, and secure credential management** are particularly important areas.

---

# 🧪 Hands-On Labs

Practical work from this section is documented inside [`labs/`](./labs/).

```text
labs/
│
├── 01_AWS_Budget_Setup.md
├── 02_Create_EC2_Web_Server.md
├── 03_Security_Groups.md
├── 04_SSH_EC2.md
├── 05_EC2_Instance_Connect.md
├── 06_EC2_IAM_Roles.md
├── 07_EC2_Purchasing_Options.md
└── 08_EC2_Launch_Types.md
```

---

# 🔒 GitHub Security

Because this repository is public, sensitive AWS information must never be committed.

Examples include:

```text
*.pem
*.key
AWS Access Keys
AWS Secret Access Keys
Passwords
Authentication Tokens
Private Credentials
```

A project `.gitignore` can include:

```gitignore
*.pem
*.key
.env
credentials
```

Screenshots should also be reviewed before publishing to ensure they do not expose sensitive information.

---

# 🎯 Key Takeaways

* Amazon EC2 provides virtual computing capacity in AWS.
* AMIs provide the information required to launch EC2 instances.
* Instance types provide different combinations of compute resources.
* User Data can automate initial EC2 configuration.
* Security Groups control permitted inbound and outbound traffic.
* Security Groups are stateful.
* Understanding common ports is essential for secure EC2 networking.
* SSH provides secure remote administration for Linux instances.
* SSH private keys must be protected.
* EC2 Instance Connect provides another connection method for supported instances.
* IAM Roles allow EC2 workloads to securely obtain AWS permissions.
* IAM Roles should follow least privilege.
* EC2 provides multiple purchasing options for different workload requirements.
* Spot Instances can provide lower-cost capacity for interruption-tolerant workloads.
* AWS Budgets can help monitor cloud spending.
* Secure EC2 architecture requires attention to networking, identity, credentials, operating-system security, and monitoring.

---
