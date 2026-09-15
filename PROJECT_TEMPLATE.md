# [Project Name] — e.g. "IAM Least Privilege"

## Objective
One or two sentences: what real-world problem does this project demonstrate?
> Example: "Overly permissive IAM policies are one of the most common causes of real cloud breaches. This project shows identifying an over-privileged role and scoping it down to least privilege."

## Scenario
Set up the situation like you're briefing someone. What did you build, and what problem did you intentionally introduce?
> Example: "I created an IAM role with `AdministratorAccess` attached to an EC2 instance profile — a common but risky shortcut — then rebuilt it with a scoped policy granting only the permissions the application actually needed."

## Steps taken
Numbered, specific, technical. This is the proof-of-work section.
1. ...
2. ...
3. ...

Include actual commands, console paths, or policy JSON where relevant (redact account IDs/ARNs).

## Findings / What broke
What did you discover? What went wrong before the fix? Screenshots go here.
- `screenshots/before.png`
- `screenshots/finding.png`

## Remediation
What did you change, and why does it fix the underlying problem (not just this one instance of it)?

## Before / After
```
BEFORE: [policy or config snippet]
AFTER:  [policy or config snippet]
```

## What this demonstrates
1-2 sentences connecting this back to a real job skill.
> Example: "This mirrors the least-privilege review work expected in cloud security analyst roles — identifying scope creep in IAM and tightening it without breaking functionality."

## Cleanup
Note that resources were torn down after the project (shows cost awareness and good lab hygiene).
