# AWS Cloud Fundamentals 
 
## Topics Covered 
 
- AWS Global Infrastructure 
- AWS Regions 
- Availability Zones (AZs) 
- AWS Management Console 
- Overview of AWS Services 
 
## AWS Regions 
 
An AWS Region is a geographical area where AWS operates infrastructure. 
 
Examples: 
- ap-south-1 — Mumbai 
- us-east-1 — N. Virginia 
- eu-west-1 — Ireland 
 
When selecting a Region, important considerations include: 
 
- Compliance requirements 
- Latency 
- Service availability 
- Pricing 
 
## Availability Zones 
 
A Region contains multiple Availability Zones. 
 
Availability Zones are separate infrastructure locations designed to provide 
high availability and fault isolation. 
 
Example: 
 
ap-south-1 
├── ap-south-1a 
├── ap-south-1b 
└── ap-south-1c 
 
Applications can be deployed across multiple AZs to reduce the impact of a 
single-AZ failure. 
 
## Region vs Availability Zone 
 
| Region | Availability Zone | 
|---|---| 
| Geographic area | Isolated location within a Region | 
| Contains multiple AZs | Belongs to one Region | 
| Selected based on latency, compliance, cost, etc. | Used for redundancy and high availability | 
 
## AWS Management Console 
 
The AWS Management Console provides a web interface for accessing and managing 
AWS services. 
 
Services can be searched and opened directly from the console. 
 
## Services Explored 
 
During the AWS Console tour, I became familiar with the AWS service categories 
and console navigation. 
 
Some major AWS services include: 
 
- EC2 — Compute 
- S3 — Object Storage 
- RDS — Relational Databases 
- VPC — Networking 
- IAM — Identity and Access Management 
- Lambda — Serverless Compute 
- CloudWatch — Monitoring 
 
## Security Connection 🔐 
 
Understanding Regions and Availability Zones is also important from a security 
perspective. 
 
- Data residency requirements can affect Region selection. 
- Multi-AZ architectures improve resilience and availability. 
- AWS services may be global or regional. 
- IAM controls who can access AWS resources. 
- AWS follows a shared responsibility model between AWS and the customer. 
 
## Key Takeaways 
 
- A Region is a geographical AWS infrastructure area. 
- Each Region contains multiple Availability Zones. 
- Multi-AZ architecture can improve availability and fault tolerance. 
- Not every AWS service is available in every Region. 
- Region selection can depend on compliance, latency, service availability, 
  and cost.
