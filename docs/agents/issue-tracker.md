# Issue tracking: GitHub

risaml, Rhein Industries' fork of saml-rs, tracks public requests and
collaboration in GitHub Issues at
<https://github.com/Rhein-Industries/risaml/issues>. Security problems are
reported privately as described in `SECURITY.md`, never in a public issue.

The upstream saml-rs project uses its own trackers; do not file risaml work
there.

## GitHub operations

Infer the repository from `git remote -v` (the `origin` remote, not the
fetch-only `upstream` remote that points at saml-rs) and use the `gh` CLI.

- **Create**: `gh issue create --title "..." --body "..."`
- **Read**: `gh issue view <number> --comments`
- **List**: `gh issue list --state open --json number,title,body,labels,comments`
- **Comment**: `gh issue comment <number> --body "..."`
- **Label**: `gh issue edit <number> --add-label "..."` or
  `--remove-label "..."`
- **Close**: `gh issue close <number> --comment "..."`

Tickets use the triage labels in `docs/agents/triage-labels.md`.

### Pull requests as a triage surface

**PRs as a request surface: no.**

Pull requests remain public code-review artifacts, but `/triage` does not
discover them as incoming requests. Resolve an explicitly supplied bare
`#<number>` with `gh pr view <number>` and fall back to
`gh issue view <number>`.
