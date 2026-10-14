# Brief template

A brief for handing one task to a coding agent. Copy it, fill in every part in angle brackets, and delete lines that do not apply. One brief, one issue, one pull request.

```text
REPO: <owner>/<repo>  (local: <path on your machine>)
BRANCH: issue-<number>-<short-slug>, from up-to-date main
ISSUE: #<number>

HOUSE RULES
- Follow skills/hackingdao-setup/rules.md.
- <any rule this repo adds>

ISOLATION
- Work only inside <repo folder>. No files anywhere else.
- Any scratch script you write deletes itself when done.

GOAL
<One or two sentences: what is true when this is done.>

PHASE 1: RECON (report, then continue)
1. <What to find or confirm before changing anything.>
2. <...>
Report what you found, then continue straight into Phase 2 in the same run.
STOP and report instead if: <condition that means the plan is wrong>.

PHASE 2: GROUPED COMMITS
Commit 1 "<type>: <summary>"
- <change>
Commit 2 "<type>: <summary>"
- <change>

VERIFY
- <Checks: lint, typecheck, tests, a manual check of the feature.>
- Flag anything you could not check, and why.

FINISH
- Push the branch and open a pull request to main with "Closes #<number>" in the body.
- Leave it open. Do not merge.
- Pull request body: what changed, what recon found, test results, what was not checked.
```
