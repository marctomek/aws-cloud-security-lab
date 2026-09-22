# S3 Misconfiguration

## Objective
Publicly accessible S3 buckets are one of the most common real-world causes of cloud data breaches. This project demonstrates deliberately creating a public S3 bucket misconfiguration, confirming it's exploitable, and remediating it back to a secure, private state.

## Scenario
I created a test S3 bucket, uploaded a placeholder file, and intentionally misconfigured the bucket by disabling Block Public Access and attaching a bucket policy granting public read access — a mistake that happens in real environments when buckets are set up quickly without reviewing default security settings. I then verified the file was publicly downloadable from the open internet, before remediating the bucket back to private and confirming the fix worked.

## Steps taken
1. Created a new S3 bucket (`marc-lab-s3-misconfig-2026`) with Block Public Access enabled by default.
2. Uploaded a placeholder test file (`test-file.txt`) containing no sensitive data.
3. Went to the bucket's Permissions tab and disabled all four Block Public Access settings, confirming the change.
4. Added a bucket policy granting `s3:GetObject` to all principals (`"Principal": "*"`) on the bucket's objects — a public-read policy.
5. Copied the object's public URL and loaded it in a browser to confirm the file was accessible without authentication.
6. Re-enabled all four Block Public Access settings and removed the public-read bucket policy.
7. Reloaded the same URL to confirm the fix — the request now returns an `AccessDenied` error instead of the file's contents.

## Findings
- **Before remediation:** the object's public URL (`https://marc-lab-s3-misconfig-2026.s3.us-east-2.amazonaws.com/test-file.txt`) loaded the file's full contents directly in the browser, with no login or credentials required — proof the bucket was genuinely exploitable by anyone with the URL.
- **After remediation:** the same URL returned:
```xml
  <Error>
    <Code>AccessDenied</Code>
    <Message>Access Denied</Message>
  </Error>
```
  confirming public access was fully revoked.

- <img width="1416" height="432" alt="before-public-access" src="https://github.com/user-attachments/assets/904fe126-04f9-4eb3-ba80-4e748ada0e19" /> — the object URL loading the file's content publicly
- <img width="1347" height="303" alt="after-access-denied" src="https://github.com/user-attachments/assets/0343d003-c9b1-414c-ac98-fd82bbacd6d9" /> — the same URL returning AccessDenied after remediation

## Remediation
Two changes were required together — removing only one would have left the bucket exposed:
1. **Block Public Access settings** re-enabled at the bucket level (all four options).
2. **Bucket policy** removed, since a public-read policy can override some Block Public Access defaults if left in place.

In a real environment, this same misconfiguration would typically be caught automatically by **AWS Config** (via the `s3-bucket-public-read-prohibited` managed rule) or surfaced as a finding in **AWS Security Hub**, rather than relying on manual URL testing — this project's Security Hub project (05) builds directly on that detection layer.

## What this demonstrates
This maps to Domain 2 (Security and Compliance) and Domain 3 (Cloud Technology and Services) of the AWS Certified Cloud Practitioner exam — specifically S3 storage concepts and the security misconfiguration detection patterns that show up constantly in real cloud security analyst work. Public S3 buckets are consistently among the most common root causes of actual reported cloud data breaches, which makes this one of the highest-value scenarios to understand hands-on rather than just conceptually.

## Cleanup
The test bucket and its contents were deleted after completing this project to avoid any lingering public-access risk or free-tier usage.
