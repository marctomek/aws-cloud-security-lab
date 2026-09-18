# IAM Least Privilege

## Objective
Overly permissive IAM policies are one of the most common causes of real cloud breaches. This project demonstrates identifying an over-privileged IAM role attached to an EC2 instance and scoping it down to least privilege.

## Scenario
I created an IAM role (`demo-overpermissive-role`) with the AWS-managed `AdministratorAccess` policy attached and assigned it to an EC2 instance — a common but risky shortcut that grants full account access when the "application" only actually needed read access to a single S3 bucket. I then rebuilt the role's permissions with a custom, scoped policy granting only the specific S3 actions required.

## Steps taken
1. Launched a free-tier EC2 instance (`lab-least-privilege-demo`) to serve as a realistic target.
2. Created an IAM role (`demo-overpermissive-role`) with EC2 as the trusted entity and attached the AWS-managed `AdministratorAccess` policy.
3. Attached the role to the EC2 instance via Actions → Security → Modify IAM role.
4. Created a throwaway S3 bucket (`marc-lab-demo-2026`) to serve as the real resource the role should have access to.
5. Wrote a custom IAM policy (`demo-s3-readonly-scoped`) in the JSON policy editor, granting only `s3:GetObject` and `s3:ListBucket` on that one bucket.
6. Removed `AdministratorAccess` from the role and attached `demo-s3-readonly-scoped` in its place.
7. Verified the role's Permissions policies tab showed only the scoped custom policy, with `AdministratorAccess` fully removed.

## Findings / What broke
Before the fix, the role granted the instance full administrative access to the entire AWS account — meaning if the instance were ever compromised, an attacker could access, modify, or delete virtually any resource in the account, not just the one S3 bucket the application actually needed.

- <img width="1413" height="723" alt="before-admin-access" src="https://github.com/user-attachments/assets/c5b23e59-f5d4-4d29-a279-a23310f67bfe" /> — role showing `AdministratorAccess` attached
- <img width="1411" height="720" alt="after-scoped-access" src="https://github.com/user-attachments/assets/e3ef1ee6-7c6b-4b12-8606-5e5f21ec6ad0" />— role showing only the scoped custom policy attached

## Remediation
Replaced the broad managed policy with a custom policy scoped to exactly two S3 read actions on one named bucket. This follows the principle of least privilege: the role can now only do what it actually needs to do, nothing more.

## Before / After

**Before** — AWS-managed policy, full admin access:
```
AdministratorAccess (AWS managed)
- Grants: Full access to all AWS services and resources
```

**After** — custom scoped policy:
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:GetObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::marc-lab-demo-2026",
        "arn:aws:s3:::marc-lab-demo-2026/*"
      ]
    }
  ]
}
```

## What this demonstrates
This mirrors the least-privilege review work expected in cloud security analyst roles — identifying scope creep in IAM and tightening it without breaking the application's actual functionality. It maps to Domain 2 (Security and Compliance) of the AWS Certified Cloud Practitioner exam, specifically access management and the principle of least privilege.

## Cleanup
The EC2 instance and throwaway S3 bucket were terminated/deleted after completing this project to avoid unnecessary free-tier usage.
