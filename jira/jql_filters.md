# Saved JQL filters

Six filters (saved with the `KV` naming prefix, e.g. *KV SECOPS Open vulnerabilities*) back the **SecOps Overview** dashboard and the team's daily views. All are shared with the space that owns the data (SecOps for SECOPS filters, the help desk for HELP, both for the cross-space filter); editing stays private.

| # | Purpose | JQL | Used by |
|---|---|---|---|
| 1 | My open work | `assignee = currentUser() AND statusCategory != Done ORDER BY priority DESC` | Personal view for anyone |
| 2 | Open vulnerabilities | `project = SECOPS AND labels = vulnerability AND statusCategory != Done ORDER BY priority DESC` | Dashboard: Filter Results gadget |
| 3 | High priority not started | `project = SECOPS AND priority in (Highest, High) AND status = "To Do"` | Sprint planning |
| 4 | Unassigned in the help desk | `project = HELP AND assignee is EMPTY AND statusCategory != Done` | Help desk triage |
| 5 | Resolved in last 7 days | `project in (SECOPS, HELP) AND resolved >= -7d` | Weekly reporting across both spaces |
| 6 | Critical open bugs | `project = SECOPS AND "Severity" = Critical AND statusCategory != Done` | Uses the custom Severity field |

## Other JQL used in the build

| Purpose | JQL |
|---|---|
| Bulk-assign items to epics without catching the epics themselves | `project = SECOPS AND labels = epic-access AND type != Epic` |
| Verify the security level (returns SECOPS-11 for the security team; errors for everyone else because the level is invisible to them) | `project = SECOPS AND level = "Security team only"` |
| Live ticket list embedded in the Confluence VPN runbook | `project in (SECOPS, HELP) AND text ~ "VPN" ORDER BY created DESC` |
| HELP queue: KV HELP Unassigned | `assignee is EMPTY AND statusCategory != Done` |
| HELP queue: KV HELP Security reports | `labels = security AND statusCategory != Done ORDER BY priority DESC, created ASC` |

**Why statusCategory instead of status:** status names differ between workflows (the Security Bug workflow adds Triage and In Review), but every status belongs to To Do, In Progress or Done, so `statusCategory` keeps filters working across workflows.
