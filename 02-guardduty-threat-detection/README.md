# GuardDuty Threat Detection

## Status: Blocked — pending AWS Support resolution

## What happened
Amazon GuardDuty (and separately, AWS Security Hub) has been inaccessible on this AWS account since setup, redirecting to an "incomplete signup" page rather than the service console. All other services used in this lab (IAM, EC2, S3, CloudTrail, IAM Access Analyzer) work normally on the same account.

## Troubleshooting steps taken
1. Confirmed the issue was specific to security/threat-detection services, not account-wide, by successfully accessing CloudTrail and other services in parallel.
2. Filed an AWS Support case. The first response incorrectly stated GuardDuty requires a "paid plan upgrade" — this is inaccurate, since all AWS accounts are pay-as-you-go by default and GuardDuty includes a built-in 30-day free trial with no separate plan required.
3. Replied to the support case with a corrected explanation and requested further investigation into a possible account-level restriction on this category of service.
4. As of this writing, the issue remains unresolved and the case is open.

## What this demonstrates
Real environments have real blockers — accounts get flagged, tools don't work as documented, and support responses aren't always accurate on the first pass. This log reflects the actual troubleshooting and escalation process, rather than a scripted, always-smooth lab exercise. See Project 05 for how the underlying learning objective (automated threat/misconfiguration detection) was still achieved via IAM Access Analyzer once Security Hub proved similarly blocked.

## Next steps
If/when AWS Support resolves the access issue, this project will be completed as originally planned: enabling GuardDuty, generating sample findings, and documenting a triage walkthrough. (shows cost awareness and good lab hygiene).
