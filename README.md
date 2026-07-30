# p6m7g8-actions/p6-gh-pr-stale

- [p6m7g8-actions/p6-gh-pr-stale](#p6m7g8-actionsp6-gh-pr-stale)
  - [Usage](#usage)
  - [Inputs](#inputs)
  - [Issues](#issues)

## Usage

```yaml
      - name: Label Stale PRs
        uses: p6m7g8-actions/p6-gh-pr-stale@main
```

## Inputs

| Input | Default | Description |
| --- | --- | --- |
| `days_before_pr_stale` | `14` | Days of PR inactivity before it is marked stale. |
| `days_before_pr_close` | `2` | Days after a PR is marked stale before it closes. |
| `include_issues` | `false` | Sweep issues as well as pull requests. |
| `days_before_issue_stale` | `60` | Days of issue inactivity before it is stale. |
| `days_before_issue_close` | `7` | Days after an issue is stale before it closes. |

Passing nothing keeps the pull request timings this action has always used.
Set a day count to `-1` to disable that step of the sweep; `-1` is the
`actions/stale` sentinel for never.

The two `days_before_issue_*` inputs are ignored unless `include_issues` is
`true`.

## Issues

This action sweeps pull requests only, which is what its name and the usage
above promise. Issue handling is available but opt-in, so a repository that
passes nothing never has its issues touched:

```yaml
      - name: Label Stale PRs and Issues
        uses: p6m7g8-actions/p6-gh-pr-stale@main
        with:
          include_issues: "true"
```

`include_issues` must be spelled `true` or `false`. Any other value fails the
step rather than being read as `false`. An empty value is the one exception: a
composite default only applies when an input is absent, so a caller wiring this
through an unset variable passes `""`, which takes the documented default.

Day counts must be integers. `actions/stale` parses them with `parseFloat`, so a
non-numeric value would otherwise become `NaN`, fall back to the generic
`days-before-*` pair, and silently switch the whole sweep off. They are
validated up front instead.
