# Triage Labels

The skills use five canonical state roles, applied as GitHub labels.

| Role | GitHub label | Meaning |
| --- | --- | --- |
| `needs-triage` | `needs-triage` | Maintainer evaluation is required |
| `needs-info` | `needs-info` | Waiting for more information |
| `ready-for-agent` | `ready-for-agent` | Ready for an AFK agent |
| `ready-for-human` | `ready-for-human` | Requires human implementation |
| `wontfix` | `wontfix` | Will not be actioned |

`/triage` operates on GitHub issues; `/to-tickets` applies `ready-for-agent`
by default.

Use exactly one canonical state-role label on a triaged issue, and replace the
previous one when the role changes; do not accumulate contradictory roles.
