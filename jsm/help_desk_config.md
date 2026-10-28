# Help desk configuration: Kestrel Valley IT Help Desk (HELP)

Jira Service Management space for clinic staff. Created from the general service template, so ITSM work types (Problem, Change) were added by hand on Day 2.

## Request types and portal groups

| Portal group | Request type | Notes |
|---|---|---|
| Report security issue | Suspicious email (phishing) | Triggers the phishing triage automation |
| Report security issue | Lost or stolen device | |
| Get help | VPN or remote access problem | Includes the optional Vendor field (added via admin request HELP-7) |
| Get help | Password reset / account locked | |
| Request something | New hire / access request | |
| Request something | New or replacement laptop | |
| Request something | Atlassian admin request | Staff request Jira configuration changes here; each one is logged with a rollback |

## SLAs

Urgent work runs on a 24/7 clock; everything else on a 9-to-5 business calendar (8h = 1 business day). Goals are evaluated top to bottom, so the most specific priority comes first.

| Priority | Time to first response | Time to resolution | Calendar |
|---|---|---|---|
| Highest | 30m | 4h | 24/7 |
| High | 2h | 8h | 9-5 |
| Medium | 8h | 24h | 9-5 |
| Low | 24h | 40h | 9-5 |
| All remaining | 8h | 24h | 9-5 |

Verified: Medium tickets logged Monday after 5 pm showed first response due Tuesday 5 pm and resolution due Thursday 5 pm, with clocks paused outside business hours.

## Queues

| Queue | Shows | Why |
|---|---|---|
| KV HELP Security reports | Open tickets labeled `security`, by priority then age | Security reports get triaged first |
| KV HELP Unassigned | Open tickets with no assignee | Nothing sits without an owner |

Both queues show SLA and Request type columns.

## Automation: KV HELP Phishing triage

| Part | Setting |
|---|---|
| Trigger | Work item created |
| Condition | Request Type = Suspicious email (phishing) |
| Actions | Priority = High; add label `security` (JSON `update` so existing labels are kept); assign to the security lead; public comment telling the reporter not to click any links |

Verified in the automation audit log: 1 success (HELP-1: High, labeled, assigned, commented) and 4 "No actions performed" for the non-phishing tickets.

```json
{ "update": { "labels": [ { "add": "security" } ] } }
```

## Tickets worked

| Key | What | Outcome |
|---|---|---|
| HELP-1 to HELP-5 | Submitted by the portal customer account across the request types | Triaged through the queues and worked with public replies and internal notes; HELP-3 resolved by the change in HELP-9 |
| HELP-7 | Atlassian admin request: add a Vendor field to VPN requests | Field created, added to the VPN form, logged in the admin change log |
| HELP-8 | Problem: recurring VPN outages for remote staff | Linked to incidents HELP-2 and HELP-3; root cause: VPN gateway firmware bug |
| HELP-9 | Normal change: upgrade firewall firmware | Approved by the test user, implemented, fixed the problem |

## Change workflow: KV HELP Change workflow

`To Do → Request approval → Waiting for approval → Approved → In Progress → Implemented → Done`, plus `Declined → Done (Resolution = Declined)`.

The trial's JSM approval workflow was read-only and the Add approval option wasn't available, so approval is enforced with a transition condition: only the work item's assignee can run **Approved**, and the change is assigned to the approver while it waits. In production this would be the JSM Approvers field or a CAB group.
