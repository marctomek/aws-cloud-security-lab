# VPC Security Group Misconfiguration

## Objective
One of the most common real-world cloud security findings is a network rule that's broader than it needs to be — most classically, SSH or RDP left open to the entire internet instead of a specific IP. This project demonstrates identifying and remediating exactly that misconfiguration on an EC2 security group.

## Scenario
I launched a free-tier EC2 instance behind a dedicated security group, deliberately configuring its inbound SSH rule to allow traffic from anywhere (`0.0.0.0/0`) rather than a specific address — the same class of misconfiguration that shows up constantly in real cloud security audits and is a frequent root cause in breach reports. I then audited the rule, tightened it to my own IP only, and verified the fix before tearing the environment down.

## Steps taken
1. Launched a `t2.micro` EC2 instance (`i-0ad85827ca29a3a21`, `us-east-2`) using Amazon Linux 2023, generating a dedicated key pair for the instance.
2. Created a new, isolated security group for this instance — `misconfig-demo-sg` (`sg-0d382cdc91537c0fa`) — rather than reusing a security group from an earlier project, so the misconfiguration stayed contained to a single throwaway resource.
3. Configured the group's inbound rule with the vulnerability in place: SSH (TCP/22) with source `0.0.0.0/0` — open to every IP address on the internet, not just mine.
4. Reviewed the security group's **Inbound rules** tab to confirm and document the exposed rule (`sgr-06e52178b27bc66bb`).
5. Remediated the finding: edited the inbound rule's source from `0.0.0.0/0` to **My IP**, scoping SSH access to a single `/32` address.
6. Re-checked the inbound rules tab to confirm the rule now reflects the restricted source.
7. Terminated the EC2 instance to avoid any ongoing cost or exposure once the demonstration was complete.

- `screenshots/inbound-rule-before.png` — inbound rules showing SSH open to `0.0.0.0/0`
- `screenshots/inbound-rule-after.png` — inbound rules showing SSH restricted to a single IP (`/32`)

## Findings
- **Security group:** `misconfig-demo-sg` (`sg-0d382cdc91537c0fa`), in VPC `vpc-0493fd3325d00d8a9`
- **Vulnerable rule:** `sgr-06e52178b27bc66bb` — SSH (TCP/22), source `0.0.0.0/0`
- **Risk:** any host on the internet could attempt to open an SSH connection to the instance. In a real environment, this is one of the first things an attacker's automated port scanner would find, and it's routinely how compromised EC2 instances get their start — via brute-force or credential-stuffing attempts against an openly exposed SSH port.
- **Remediation applied:** source restricted to my own IP address as a `/32` CIDR, so only a single known address can attempt SSH — the same rule an internal security review or automated remediation script would apply.
- **Verification:** re-inspecting the inbound rules confirmed `0.0.0.0/0` was fully replaced, with no other rules on the group carrying the same exposure.

## What this demonstrates
This is the "find the overly permissive rule, narrow it, verify the fix" workflow that sits at the core of cloud network security review — whether performed manually, as here, or automatically by a tool like AWS Security Hub or GuardDuty (see Project 02), which would normally flag a `0.0.0.0/0` ingress rule as a finding requiring exactly this remediation. It maps to Domain 2 (Security and Compliance) of the AWS Certified Cloud Practitioner exam, specifically the shared responsibility model as it applies to network-level access controls — AWS secures the infrastructure, but configuring security groups correctly is squarely the customer's responsibility.

It also reinforces a theme running through this whole repo: cloud security is rarely about a single control. Identity (Project 01), audit logging (Project 03), storage permissions (Project 04), and network exposure (this project) each close off a different attack surface, and a real security posture requires getting all of them right together.

## Cleanup
The EC2 instance (`i-0ad85827ca29a3a21`) was terminated immediately after the remediation was verified, and the dedicated security group was removed along with it, so no resources from this project remain running in the account.
