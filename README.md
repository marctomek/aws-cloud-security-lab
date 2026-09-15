# AWS Cloud Security Lab

Hands-on projects documenting my transition into cloud security, built in a dedicated AWS free-tier account. Each project follows the same pattern: **deploy → misconfigure/attack → detect → remediate → document.**

**Background:** Former [DoD/military — edit this] with a security foundation, currently building AWS-specific hands-on experience alongside AWS Cloud Practitioner and Solutions Architect Associate certifications.

## Projects

| # | Project | AWS Services | Status |
|---|---------|---------------|--------|
| 1 | [IAM Least Privilege](./01-iam-least-privilege) | IAM | ⬜ Not started |
| 2 | [GuardDuty Threat Detection](./02-guardduty-threat-detection) | GuardDuty | ⬜ Not started |
| 3 | [CloudTrail Investigation](./03-cloudtrail-investigation) | CloudTrail, S3, CloudWatch | ⬜ Not started |
| 4 | [S3 Misconfiguration](./04-s3-misconfiguration) | S3, Security Hub, Config | ⬜ Not started |
| 5 | [Security Hub Remediation](./05-security-hub-remediation) | Security Hub, EventBridge, Lambda | ⬜ Not started |

Update status to 🟡 In Progress / ✅ Complete as you go — this table is the first thing anyone opening the repo sees.

## Why this repo exists

I don't have civilian cloud security work history yet. This lab is how I prove hands-on capability instead of just claiming it — every project here was actually built, broken, detected, and fixed by me, not copied from a tutorial. Full writeups are in each project folder.

## Certifications in progress

- [ ] AWS Certified Cloud Practitioner (CLF-C02)
- [ ] AWS Certified Solutions Architect – Associate (SAA-C03)

## Setup notes

- All work done in an isolated AWS free-tier account with billing alarms enabled
- No production data or credentials — all resources are lab-only and torn down after each project unless noted
