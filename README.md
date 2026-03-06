# workflow-templates

A collection of reusable GitHub Actions workflows.

## Available Workflows

| Workflow | Description |
|----------|-------------|
| [Check PR Issue Link](#check-pr-issue-link) | Validates that pull request descriptions reference a valid GitHub issue |

## Check PR Issue Link

Ensures every pull request includes a closing keyword that links to a valid GitHub issue
(e.g. `Closes #42`). When a valid link is missing, the workflow posts a warning comment and
applies a `needs-issue-link` label. Once the PR description is corrected, the comment and
label are automatically removed.

### Supported keywords

All keywords are **case-insensitive** and may optionally include `#` before the issue number:

`closes`, `close`, `fixes`, `fix`, `resolves`, `resolve`

### Behavior

| Scenario | Comment | Label |
|----------|---------|-------|
| No issue reference found | Warning posted | `needs-issue-link` added |
| Referenced numbers are invalid (not found or are PRs) | Warning posted | `needs-issue-link` added |
| Mix of valid and invalid references | Heads-up posted | Label removed |
| All references valid | Previous warning removed | Label removed |

### Usage

To call this workflow from another public repository, create a caller workflow in your repo:

```yaml
# .github/workflows/check-pr-issue-link.yml
name: Check PR Issue Link
on:
  pull_request:
    types: [opened, edited, reopened, synchronize]

permissions:
  pull-requests: write
  issues: write

jobs:
  check:
    uses: StacklokLabs/workflow-templates/.github/workflows/check-pr-issue-link.yml@main
```

### Prerequisites

- The calling workflow must declare the required permissions:
  - `pull-requests: write`
  - `issues: write`

