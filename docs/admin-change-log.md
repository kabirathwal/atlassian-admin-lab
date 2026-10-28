# Admin change log

Every configuration change to the Kestrel Valley Atlassian site: what changed, why, how to undo it, and how it was tested. The same log is published as a page in the Confluence space **IT Ops Knowledge Base (ITOPS)**, and it lines up with Jira's audit log (see `screenshots/7-confluence-and-audit/`).

**41 changes logged.** Known gaps are recorded in the row where they happened, not hidden.

## Day 1 (Oct 22 to 26, 2026): Jira, access control, help desk

| Date | Space | Change | Reason | Rollback | Tested? |
|---|---|---|---|---|---|
| Oct 22, 2026 | SECOPS | Created company-managed Scrum space SECOPS; imported 24 work items from CSV | Set up the security team's work tracking | Delete the imported items, or the space | Yes: 24 items in backlog |
| Oct 22, 2026 | SECOPS | Created 5 epics and assigned work items with bulk change (JQL excluded epics: type != Epic) | Group work by goal | Clear the Parent field | Yes: each epic shows its children |
| Oct 22, 2026 | SECOPS | Created components Endpoint, Identity, Network; component lead = me; default assignee = Component lead | Route work by system area | Delete the components | Yes: unassigned item went to the lead |
| Oct 22, 2026 | Global | Added Task and Bug to the Story Points field context | Story points from the CSV import were dropped for those types | Remove Task and Bug from the context | Yes: points now show on Tasks and Bugs |
| Oct 22, 2026 | Site | Added Jira Service Management (Premium trial) to the site | Needed for the IT help desk, approvals and Assets | Cancel the JSM subscription | Yes: JSM available in the app switcher |
| Oct 22, 2026 | HELP | Created JSM space Kestrel Valley IT Help Desk (key HELP). Note: it was created from the general service template, so its work types are Submit a request or incident, Ask a question, Emailed request and Task (no Incident, Problem or Change) | IT help desk for clinic staff | Move the space to trash | Yes: space and queues visible |
| Oct 22, 2026 | Site | Moved the auto-created Demo service space to trash | Remove sample data from reports, filters and Rovo | Restore it from the trash | Yes: no longer listed in Spaces |
| Oct 22, 2026 | SECOPS | Software Simplified Workflow: added "Update field: Resolution = Done" on the Done transition. Known limitation: the editor can't clear Resolution, so reopened items keep it (planned fix: automation rule to clear it when an item leaves Done) | Done items had no resolution, so reports showed 0 resolved and resolved searches returned nothing | Remove the rule from the Done transition | Yes: Created vs Resolved chart now shows 3 resolved |
| Oct 22, 2026 | SECOPS | Shared all 6 KV filters and the SecOps Overview dashboard (viewers: SecOps for SECOPS filters, Kestrel Valley IT Help Desk for the HELP filter, both for the cross-space filter; editors: private) | Make the dashboard usable by the whole team; gadgets need shared filters to display | Set viewers back to Private | Yes: Viewers column shows SecOps, All roles |
| Oct 22, 2026 | Global | Created custom field Severity (single-select: Critical, High, Medium, Low; no default). Global context left as is: Jira no longer allows restricting it | Rate the technical severity of security bugs, separately from Priority | Delete the field (or remove it from the field scheme) | Yes: options and no default confirmed |
| Oct 22, 2026 | SECOPS | Created KV SECOPS Field Scheme (copy of the default, so other spaces are unaffected) and associated it with SecOps; Severity available and required for Bug only, with Bug-specific guidance text | Make every security bug carry a severity rating | Re-associate SecOps with the default field scheme | Yes: required on Bug create; absent on Story |
| Oct 22, 2026 | SECOPS | Added Severity to the SECOPS Bug screen (create, edit, view) | Field existed but didn't appear on the Bug form | Remove Severity from the screen | Yes: Severity shows on the create form and the bug page |
| Oct 22, 2026 | Global | Created space role Security Lead (can approve and close security bugs, set security levels) | Role-based approval for security work | Delete the role | Yes: role listed under Space roles |
| Oct 22, 2026 | SECOPS | Added myself to the Security Lead role in SECOPS (kept Administrator) | Someone must hold the role to approve bugs | Remove me from the role | Yes: shown under Space settings → People |
| Oct 22, 2026 | Global | Created workflow KV SECOPS Security Bug workflow: To Do → Triage → In Progress → In Review → Done, plus Changes needed (In Review → In Progress); no "any status" transitions. Rules: Triage validator requires Severity; Start work assigns the current user; Approve restricted to the Security Lead role and sets Resolution = Done | Enforce rating, ownership and lead approval for security bugs | Delete the workflow (after unmapping it) | Yes: Triage blocked without Severity (SECOPS-13); front-desk PC bug ran Triage → Start work (auto-assigned) → Submit → Changes needed → Submit → Approve → Done with Resolution = Done. Member-role test user saw Submit but no Approve |
| Oct 22, 2026 | SECOPS | Workflow scheme: mapped Bug → KV SECOPS Security Bug workflow; other work types keep the default workflow; existing bug statuses mapped to matching names. SECOPS board columns: mapped the new Triage status to To Do and In Review to In Progress | Apply the security process to bugs only | Map Bug back to the default workflow | Yes: Bugs follow the new workflow; Stories and Tasks keep the default |
| Oct 26, 2026 | Global | Created space role Viewer (read-only: browse and search, no create, edit or transition) | Least-privilege role for read-only users | Delete the role | Yes: role listed under Space roles |
| Oct 26, 2026 | Site | Invited test user (kabirathwal+test) with Jira User and JSM Agent access; no admin access | Test account to verify permissions, security levels and approvals | Remove the user | Yes: test user signed in |
| Oct 26, 2026 | SECOPS | Added the test user to SECOPS with the Viewer role. Help desk: the customer account had been added to the agent team by mistake; removed it, and added the test user (TEST123) as Service Desk Team (agent). Found with the permission helper when TEST123 couldn't be assigned | Ordinary read-only user for SECOPS permission tests; agent access so they can act as an approver in the help desk | Remove the user from the space | Yes: shown as Viewer under Space settings → People |
| Oct 26, 2026 | Global | Created space role Member (create, edit, assign, transition, manage sprints; no delete or space settings) | Contributor role between Viewer and Security Lead | Delete the role | Yes: role listed under Space roles |
| Oct 26, 2026 | Global | Created KV SECOPS Permission Scheme (copy of the Default Permission Scheme). Removed "Any logged in user" from every permission; granted to space roles only (see permission matrix below). Kept Atlassian system roles (atlassian-addons-project-access, jira-guest-member) for apps and guest access. Left Edit Workflows to Jira admins only (permission warns it exposes global config) | Least privilege: only people with a SECOPS role can see or change SECOPS work | Switch SECOPS back to the Default Permission Scheme | Yes: Permission helper shows the test user (Viewer) denied Edit Issues: not in Administrators, Security Lead or Member |
| Oct 26, 2026 | SECOPS | Applied KV SECOPS Permission Scheme to SECOPS | Replace the default open permissions | Use a different scheme → Default Permission Scheme | Yes: SECOPS now uses KV SECOPS Permission Scheme (Space settings → Permissions) |
| Oct 26, 2026 | Global | Created work item security scheme KV SECOPS Security Levels with level "Security team only"; level members: space roles Security Lead and Administrators | Hide sensitive security work (secrets exposure, breach investigations) from everyone else | Delete the level, or remove the scheme from SECOPS | Yes: I (Administrator and Security Lead) can select the level on SECOPS work items |
| Oct 26, 2026 | SECOPS | Attached KV SECOPS Security Levels to SECOPS; set SECOPS-11 (chat-api secrets bug) to Security team only | Restrict the most sensitive finding to the security team | Set SECOPS-11's security level to None, or detach the scheme | Yes: project = SECOPS AND level = "Security team only" returns SECOPS-11 for me; for the test user the same query errors with "The value 'Security team only' does not exist for the field 'level'": the level itself is invisible to them |
| Oct 26, 2026 | SECOPS | Temporarily moved the test user from Viewer to Member to test the Approve condition, then back to Viewer | Prove only the Security Lead role can approve | n/a (role restored) | Yes: as Member, saw Submit but no Approve; back to Viewer afterward |
| Oct 26, 2026 | HELP | Created 7 request types with descriptions and instructions (forms use the Summary field only; no extra custom fields): Suspicious email (phishing), Lost or stolen device, VPN or remote access problem, Password reset / account locked, New hire / access request, New or replacement laptop, Atlassian admin request. Portal groups: Report security issue, Get help, Request something. Added portal customer account (kabirathwal+customer) | Give staff a clear portal and route each request to the right queue | Delete the request types | Yes: customer submitted 5 tickets (HELP-1 to HELP-5) through the portal |
| Oct 26, 2026 | HELP | SLA goals by priority. Time to first response: Highest 30m (24/7), High 2h, Medium 8h, Low 24h, remaining 8h (9-5 calendar). Time to resolution: Highest 4h (24/7), High 8h, Medium 24h, Low 40h, remaining 24h (9-5) | Urgent issues run on a 24/7 clock; everything else on business hours (8h = 1 business day) | Restore the template's default goals | Yes: Medium tickets logged Monday after 5 pm showed first response due Tuesday 5 pm and resolution Thursday 5 pm, with clocks paused outside business hours |
| Oct 26, 2026 | HELP | Created queues KV HELP Security reports (labels = security, open, ordered by priority then age) and KV HELP Unassigned (assignee is EMPTY, open); added SLA and Request type columns | Let agents triage security reports first and catch unowned tickets | Delete the queues | Yes: Security reports showed only the phishing ticket; Unassigned showed the other 4 until they were assigned |
| Oct 26, 2026 | HELP | Automation KV HELP Phishing triage: when a work item is created with Request Type = Suspicious email (phishing), set Priority = High, add label security (JSON update so existing labels are kept), assign to me, post a public comment telling the reporter not to click links | Fast, consistent phishing response; feeds the Security reports queue | Turn the rule off or delete it | Yes: audit log shows 1 Success (HELP-1: High, labeled, assigned, commented) and 4 "No actions performed" for non-phishing tickets |
| Oct 26, 2026 | HELP | Added the Service Request with Approvals work type to the help desk's work item type scheme | Prepare for approval-based requests | Remove it from the scheme | Yes: listed under the space's work item types. Its workflow is a read-only system workflow, so approvals need a copied workflow (planned for Day 2 change approvals) |
| Oct 26, 2026 | Global | Created custom field Vendor (short text, optional) via admin request HELP-7. Help desk field scheme: available on Submit a request or incident; not required. Added to the VPN or remote access problem request form. Optional field, so added to the shared default scheme rather than copying it | Track third-party vendors on VPN issues, as requested in HELP-7 | Remove Vendor from the form and field scheme, then delete the field | Yes: Vendor appears on the VPN request form in the portal |

## Day 2 (Oct 26, 2026): problem and change, Assets, Confluence

| Date | Space | Change | Reason | Rollback | Tested? |
|---|---|---|---|---|---|
| Oct 26, 2026 | Global | Created work item types Problem and Change (standard); added both to the help desk's work item type scheme | The help desk came from the general service template, which has no ITSM work types | Remove them from the scheme, then delete the types | Yes: Problem and Change available when creating help desk work items |
| Oct 26, 2026 | Global | Created workflow KV HELP Change workflow: To Do → Request approval → Waiting for approval → Approved → In Progress → Implemented → Done, plus Declined (→ Done, Resolution = Declined). Approved restricted to the work item's assignee; the change is assigned to the approver during approval | The JSM approval workflow was read-only and no Add approval option was available, so approval is enforced by a transition condition (in production: Approvers field or a CAB group) | Map Change back to the default workflow, then delete this workflow | Yes: with the change assigned to Test 123, I saw only Declined, not Approved; Test 123 then approved it |
| Oct 26, 2026 | HELP | Help desk workflow scheme: mapped Change → KV HELP Change workflow and published; migrated the existing Change ticket (Open/Reopened → To Do, Resolved/Closed → Done). The first attempt stayed inactive because the scheme draft was never published | Apply the change process to Change tickets only | Map Change back to the default workflow | Yes: KV HELP Change workflow now listed under Active workflows; HELP-9 picked it up |
| Oct 26, 2026 | HELP | Created problem HELP-8 "Recurring VPN outages for remote staff"; linked incidents HELP-2 and HELP-3; root cause noted (VPN gateway firmware bug) | Track the root cause behind repeated VPN incidents | n/a (work item) | Yes: HELP-8 shows HELP-2, HELP-3 and HELP-9 as linked |
| Oct 26, 2026 | HELP | Normal change HELP-9 "Upgrade firewall firmware" with risk, impact, implementation plan, backout plan and window; approved by Test 123; implemented; linked to HELP-8 and HELP-3. HELP-3 resolved referencing the change | Planned, approved fix for the problem's root cause | n/a (work item) | Yes: history shows Test 123 moved Waiting for approval → In Progress; I moved In Progress → Done with Resolution = Done |
| Oct 26, 2026 | Site | Created Assets schema Kestrel Valley IT (key KVIT) with object types Server, Application and Laptop (Laptop unused) | Inventory of clinic IT assets and their dependencies (CMDB) | Delete the schema | Yes: schema tree shows Server (2), Application (2) |
| Oct 26, 2026 | Site | Added attribute Runs on (Object → Server, reference type Dependency) to Application; created objects Server-01, VPN Gateway, Remote Access VPN (runs on VPN Gateway), EHR System (runs on Server-01) | Record which applications depend on which servers, for impact analysis | Delete the objects and the attribute | Yes: VPN Gateway shows an inbound Dependency reference from Remote Access VPN |
| Oct 26, 2026 | Global | Created Assets objects field Affected asset, pointed at the KVIT schema (displays Name); added to the help desk field scheme. Known gap: not yet showing on help desk work items, so HELP-3 isn't linked to VPN Gateway (next step: add it to the request type's issue view) | Let tickets record which asset they affect | Remove from the field scheme, then delete the field | Partly: field configured; not visible on HELP-3 yet |
| Oct 26, 2026 | Site | Added Confluence (trial) to the site | Knowledge base for runbooks, postmortems and admin records | Cancel the Confluence subscription | Yes: Confluence in the app switcher |
| Oct 26, 2026 | ITOPS | Created Confluence space IT Ops Knowledge Base (key ITOPS), Knowledge base purpose, default access. Pages: VPN runbook (with a live Jira list across SECOPS and HELP, label runbook), postmortem linked from HELP-8, and this admin change log | Keep IT knowledge next to the work it describes | Archive or delete the space | Yes: runbook's Jira macro shows 8 live work items |

## Permission matrix: KV SECOPS Permission Scheme

✓ = granted to that space role. Every permission also keeps the system role atlassian-addons-project-access (for app integrations). "Any logged in user" was removed everywhere.

| Permission | Administrators | Security Lead | Member | Viewer |
|---|---|---|---|---|
| **Space** |  |  |  |  |
| Browse Projects | ✓ | ✓ | ✓ | ✓ |
| View Read-Only Workflow | ✓ | ✓ | ✓ | ✓ |
| Manage sprints | ✓ | ✓ | ✓ |  |
| View Development Tools | ✓ | ✓ | ✓ |  |
| View aggregated data | ✓ | ✓ |  |  |
| Manage Versions (Releases) | ✓ |  |  |  |
| Administer Projects | ✓ |  |  |  |
| Manage Issue Layouts | ✓ |  |  |  |
| Edit Workflows | Jira admins only |  |  |  |
| **Work items** |  |  |  |  |
| Create / Edit / Link | ✓ | ✓ | ✓ |  |
| Assign / Assignable User | ✓ | ✓ | ✓ |  |
| Transition / Resolve / Close | ✓ | ✓ | ✓ |  |
| Schedule (due dates) | ✓ | ✓ | ✓ |  |
| Set Issue Security | ✓ | ✓ |  |  |
| Delete / Move / Archive / Restore archived / Modify Reporter | ✓ |  |  |  |
| **Watchers** |  |  |  |  |
| View Voters and Watchers | ✓ | ✓ | ✓ | ✓ |
| Manage Watchers | ✓ |  |  |  |
| **Comments, attachments, worklogs** |  |  |  |  |
| Add comments / Create attachments / Work on issues | ✓ | ✓ | ✓ |  |
| Edit or delete own comments, attachments, worklogs | ✓ | ✓ | ✓ |  |
| Edit or delete all comments, attachments, worklogs | ✓ |  |  |  |

**Why roles, not people:** one scheme can serve many spaces; each space decides who fills each role, so access follows the job, not the person.
