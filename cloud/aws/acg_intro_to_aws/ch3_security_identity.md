# Introduction
- Security: You need to protect your systems, and also *detect* when there have been
  attempts to attack them
- Identity: Ensuring that only people who should have access to your systems can access them.

## Data Protection
- Macie - detect secrets and sensitive data
- KMS
- CloudHSM
- ACM (AWS Certificate Manager)
- Secrets Manager

## Infrastructure Protection
- Shield - to protect against (D)DoS attacks
- WAF - To filter malicious web traffic
- Firewall Manager

## Threat Detection
- GuardDuty - automatically detects threats
- Inspector - vulnerability management service that scans EC2, ECR and Lambda for software
  vulnerabilities and unintended network exposure
- Config
- CloudTrail

## Identity Management
- IAM
- SSO
- Cognito - identity *inside* applications
- Directory Service - for managing MS Active Directory from AWS
- Organisations

# IAM
- Users and groups
- Policies can either allow or deny access to other AWS services or resources
- It's free to use

## IAM Users
- Root user created by default
- IAM users can be created afterwards, which can also be used to log in to the AWS account. But
  those users can have more fine grained access to specific services or resources. 
- You can put users in a group, which can have an associated policy (so you don't need to create an
  indvidual "Developer" policy for each of your developers, for example)

## IAM Policies
- Statements always (?) have three things:
  - Effect - either an `"Allow"` or `"Deny"`
  - Action - which AWS APIs the statement applies to
  - Resource - which AWS resources the statement applies to
- Policies are deny by default

## IAM Roles
- Roles are like users or groups in that they can have policies attached to them to define what they
  can acces
- The difference is that users **and services** can assume roles (not just users).
  - It also allows for cross-account access

# Directory Service
- Directories are databases of user accounts (usernames and passwords, alongside other information),
  usually used by organisations which have members that need to log in to the org's systems
  - Directories can also implement security policies and distribute configurations for specific
    groups of users
- Microsoft's Active Directory is an implementation of this for Windows
- AWS Directory Service is a managed service that implements MS AD
  - Also has Simple Active Directory if you don't need all of MS' extensions
  - Provides directories without requiring you to run the servers
    - This improves reliability and resilience against outages - will automatically have failover
      systems
  - AD Connector - use directory credentials to log in to applications you host on AWS
  - You can also use directory credentials to directly use specific AWS services, including:
    - The management console itself
    - Chime (video conferencing)
    - Connect and EC2 Instances
    - RDS
