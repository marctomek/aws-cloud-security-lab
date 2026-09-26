# Security Hub / Access Analysis Remediation

## Objective
Automated security posture tools continuously check an AWS account for misconfigurations and unintended external access, catching issues a manual review might miss. This project demonstrates setting up that kind of automated detection and validating that prior remediation work (Projects 01 and 04) holds up under independent scrutiny.

## Scenario
The original plan for this project was AWS Security Hub. However, Security Hub — like GuardDuty — was inaccessible on this account due to an unresolved account-level restriction (see note below). Rather than stay blocked, I pivoted to **IAM Access Analyzer**, a free, closely related AWS security service that continuously scans resource policies (IAM roles, S3 buckets, KMS keys, and more) for access granted to anyone outside the account's trust zone.

## Steps taken
1. Attempted to enable AWS Security Hub — redirected to an account "incomplete signup" page, the same restriction affecting GuardDuty (Project 02).
2. Filed an AWS Support case; the first response incorrectly claimed GuardDuty/Security Hub require a separate "paid plan" — this is inaccurate, as all AWS accounts are pay-as-you-go by default and these services offer built-in free evaluation. Pushed back with a corrected explanation and requested further investigation.
3. Briefly evaluated AWS Config as an alternative but declined it due to its per-evaluation cost, preferring a genuinely free option for this lab.
4. Created an IAM Access Analyzer (finding type: **Resource analysis – External access**, zone of trust: current account) — confirmed free ("no additional cost") before creating.
5. Let the analyzer run for several days, continuously scanning the account's resource policies.
6. Reviewed **Resource analysis** results: 0 resources with active findings.

## Findings
- **Active findings: 0** — no resources in the account currently grant access to anyone outside the account.
- This is meaningful, not just an empty result: it confirms that the IAM role scoped down in Project 01 and the S3 bucket remediated (then deleted) in Project 04 left no lingering external-access exposure, verified independently by a dedicated AWS security tool rather than by manual inspection alone.

- <img width="1414" height="834" alt="access-analyzer-clean-scan" src="https://github.com/user-attachments/assets/ae303e37-8a1c-448f-b091-d424fff4fea5" /> — the Resource analysis dashboard showing 0 active findings

## What this demonstrates
This maps to Domain 2 (Security and Compliance) of the AWS Certified Cloud Practitioner exam — specifically identifying AWS access management capabilities and the tools available for continuous security monitoring. It also demonstrates a real, unglamorous but important skill: **working around a tooling/account blocker without losing the underlying learning objective.** When the "textbook" service (Security Hub) wasn't accessible, the goal — verifying no unintended external access exists — was still achieved through an equivalent, freely available AWS-native tool.

## Note on GuardDuty / Security Hub access issue
Both GuardDuty (Project 02) and Security Hub remain inaccessible on this AWS account, redirecting to an incomplete-signup page. This affects only threat-detection-category services; core services (IAM, EC2, S3, CloudTrail, IAM Access Analyzer) all function normally. An AWS Support case is open and unresolved as of this writing. This is documented here transparently as an example of real-world troubleshooting and escalation, rather than omitted from the portfolio.

## Cleanup
No cleanup required — IAM Access Analyzer is a persistent, no-cost monitoring tool and was left running for ongoing account visibility. were torn down after the project (shows cost awareness and good lab hygiene).
