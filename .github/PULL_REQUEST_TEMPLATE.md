## What
<!-- Brief summary of what changed -->

## Why
<!-- Jira ticket: https://your-org.atlassian.net/browse/SCRUM-### -->

## How to Test
<!-- Steps a reviewer can follow to verify the change locally -->
1.
2.

---

## Author Checklist

- [ ] Branch is up to date with the parent epic branch (or `main` for epic-level PRs)
- [ ] Code builds locally without errors
- [ ] All existing tests pass
- [ ] New logic is covered by tests (or a Jira sub-task exists for test coverage)
- [ ] No debug output, dead code, or untracked TODO comments left in
- [ ] No secrets or credentials committed — use `.env.example` for new environment keys
- [ ] PR title follows the `SCRUM-### Description` format
- [ ] PR is targeting the correct base branch (epic branch for stories/tasks, `main` for epics)

## Reviewer Checklist

- [ ] Logic is correct and matches the ticket scope
- [ ] No security issues introduced (auth checks, input validation, no secrets in code)
- [ ] Code follows existing patterns and naming conventions in the module
- [ ] Tests are meaningful and cover the intended behavior
