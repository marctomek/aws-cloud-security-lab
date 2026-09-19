# CloudTrail Investigation

## Objective
When something goes wrong in an AWS account — a misconfiguration, a deleted resource, unexpected access — the first question is always "who did this, and when?" This project demonstrates using AWS CloudTrail to trace account activity back to a specific identity, action, and timestamp.

## Scenario
I set up a CloudTrail trail (`lab-management-trail`) to log management events across my account, generated a mix of real activity by navigating IAM, S3, and EC2, and then investigated the resulting event log — the same workflow an analyst follows when reconstructing what happened in an account during an incident or audit.

## Steps taken
1. Navigated to CloudTrail and created a trail (`lab-management-trail`), storing logs in a dedicated S3 bucket, with multi-region logging and log file validation enabled.
2. Generated activity by opening IAM (viewing my `marc-admin2` user and the `demo-overpermissive-role` from Project 01), S3 (viewing my bucket list), and EC2 (viewing instances).
3. Waited a few minutes for CloudTrail to ingest the activity, then opened **Event history**, which showed 41 logged events without needing to wait on the custom trail — Event history captures the last 90 days of management events by default, even before a custom trail exists.
4. Selected the `CreateTrail` event (the action of setting up the trail itself) and reviewed its full JSON event record.

## Findings
The event log confirmed exactly who performed each action, when, and from where. For the `CreateTrail` event specifically:

- **Who:** `marc-admin2` (IAM user, not root) — `arn:aws:iam::805782534182:user/marc-admin2`
- **When:** `2026-09-19T21:44:29Z` (UTC)
- **From where:** source IP `129.222.147.20`, via the AWS Console (Chrome on macOS)
- **MFA status:** `mfaAuthenticated: true` — confirms the session was authenticated with MFA, which matters when ruling out a compromised-credential scenario during a real investigation
- **What changed:** `readOnly: false` — this was an account-modifying action, not just a view, which is exactly the category of event an analyst scrutinizes most closely
- **Result:** the trail was created successfully, confirmed by the `responseElements.trailARN` in the response

- <img width="1415" height="710" alt="event-history-list" src="https://github.com/user-attachments/assets/2fc51179-4f76-4ad8-8d7f-a45888bd6184" /> — the Event history table showing 41 logged events
- <img width="1415" height="831" alt="event-detail-createtrail" src="https://github.com/user-attachments/assets/0d1fd98d-41a8-4e84-b941-9dea8019ec49" /> — the full JSON detail for the `CreateTrail` event

## What this demonstrates
This is the core "who did what, when" investigative workflow used in real cloud security incident response and auditing — tracing an action back to a specific IAM identity, verifying whether MFA was in use, and checking the source of the request. It maps to Domain 2 (Security and Compliance) of the AWS Certified Cloud Practitioner exam, specifically the governance and audit logging competency area.

It also directly ties back to Project 01: the `mfaAuthenticated: true` field in this event is real evidence that the MFA setup completed there is now protecting every subsequent action in this account.

## Cleanup
No resources require teardown for this project — CloudTrail's Event history is free and always-on, and the custom trail can remain active as ongoing logging for the rest of the lab work in this repo.
