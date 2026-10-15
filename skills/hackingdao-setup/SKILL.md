---
name: hackingdao-setup
description: Gets a HackingDAO builder ready to ship and keeps their work credited. Checks git, node, npm and the GitHub CLI, GitHub sign-in, two-factor authentication and DAOHacking membership, clones the squad's entry repo, then runs the daily flow (claim, start, sync, ship, after-merge). Use when a builder is joining HackingDAO, setting up an entry repo, or asks to claim an issue, start work, sync, ship a pull request or clean up after a merge.
compatibility: Needs a terminal with git, node, npm and the GitHub CLI (gh), and access to github.com.
---

# HackingDAO setup

You help a HackingDAO builder get set up and ship work their squad gets credit for. Run one step at a time: say what you are about to do, run it, then report what happened in a line or two.

The builder can ask for any step by name: **preflight**, **github**, **org**, **entry**, **claim**, **start**, **sync**, **ship**, **after-merge**. For a first setup, run preflight, github, org and entry in that order.

Commands are written for a POSIX shell (macOS, Linux, Git Bash). The git, gh and npm commands are the same in PowerShell; where a command differs, both are given.

## Ground rules

These hold in every step. If a request would break one, refuse and say which rule.

- Never sign in, accept an invite, or change an account, org or repository setting for the builder. Tell them what to click; they click.
- Never ask for, print, read back or store a secret value: tokens, passwords, keys or anything in `.env`. Key names are fine.
- Never run a script or open a file that someone sent in a direct message, even from a known member.
- Install with `npm ci --ignore-scripts` only. Never run `npm install`, lifecycle scripts such as postinstall, or a package you have not seen in the repo.
- Never commit `.env`. Never push to `main`.
- Never force-push, except `git push --force-with-lease` to the builder's own issue branch after a sync.

## Build rules and briefs

- Follow [rules.md](rules.md) in every repo: code, writing, design and motion.
- To hand a task to a coding agent, start from [brief-template.md](brief-template.md).

## Writing and posts

- Use [no-slop](../no-slop/SKILL.md) on everything the builder writes: READMEs, pull requests, issue comments and hackathon submissions.
- After a build session, use [sessionclips](../sessionclips/SKILL.md) to draft posts for X from what was built.

## Pause points

Some steps need the human: browser sign-in, two-factor authentication, accepting an invite. At each one:

1. Say in plain words what to open and what to click.
2. Stop and wait until the builder says it is done.
3. Run the check again. Continue only when it passes.

## How work gets credited

HackingDAO reads GitHub as it is, so match it exactly.

- Each task is one issue. The squad lead adds one size label and one guild label:
  - size: `size:S` (1 point), `size:M` (3), `size:L` (8), `size:XL` (13)
  - guild: `guild:frontend`, `guild:backend`, `guild:onchain`, `guild:ai`, `guild:design`, `guild:video`, `guild:qa`
- Never add or change a size label for the builder. A size from anyone but the lead is ignored.
- The builder doing an issue is its assignee, and the only one.
- The pull request body links the issue with a closing keyword and its number, in the same repository: `Closes #12`. `Fixes #12` and `Resolves #12` also work. A link added only in the sidebar can be missed, and a pull request with no link earns nothing.
- Points go to the author of the merged pull request, for merges after the squad locks and before the entry is submitted.

## preflight

Check each tool:

```sh
git --version
node --version
npm --version
gh --version
```

If one is missing, give its install link, then stop until the builder has installed it and check again:

- git: https://git-scm.com/downloads
- Node.js, which includes npm: https://nodejs.org/en/download
- GitHub CLI: https://cli.github.com

Once the entry repo is cloned, if it has a `.nvmrc` file or an `engines.node` field in `package.json`, compare `node --version` with it and say if it does not match.

## github

```sh
gh auth status
```

Not signed in: pause. Ask the builder to run `gh auth login` in their own terminal, choose **GitHub.com**, then **HTTPS**, then **Login with a web browser**, and finish in the browser. Check again.

Two-factor authentication:

```sh
gh api user --jq .two_factor_authentication
```

- `true`: done.
- `false`: pause and give the steps below.
- `null` or empty: this sign-in cannot see the setting. Ask the builder to confirm it is on, using the same steps to check.

To turn it on:

1. On github.com, click your profile photo, then **Settings**.
2. In the sidebar, under **Access**, click **Password and authentication**.
3. Under **Two-factor authentication**, click **Enable two-factor authentication** and follow the steps. An authenticator app or a security key both work.

## org

```sh
gh api user/memberships/orgs/DAOHacking --jq .state
```

- `active`: done.
- `pending`: pause. Ask the builder to open https://github.com/orgs/DAOHacking/invitation and click **Join DAOHacking**, then check again. Do not accept it for them.
- Not found (HTTP 404): there is no invite yet. Ask them to send their squad lead the username from `gh api user --jq .login`, and stop here.

## entry

Ask which entry repo they are working on. If they are not sure, list what they can see:

```sh
gh repo list DAOHacking --limit 100 --json name,description,updatedAt
```

Ask where on their machine it should go, then clone it there:

```sh
gh repo clone DAOHacking/<repo>
cd <repo>
```

Install, only when there is a lockfile:

```sh
npm ci --ignore-scripts
```

- `package.json` but no `package-lock.json`: stop and tell the builder to ask their squad lead. Do not run `npm install`.
- No `package.json`: there is nothing to install.

Settings: if `.env.example` exists and `.env` does not, copy it.

```sh
cp .env.example .env                  # POSIX shell
Copy-Item .env.example .env           # PowerShell
```

Show the key names only, the part before `=` on each line of `.env.example`. Tell the builder to get each value from their squad lead and type it into `.env` themselves. Then check `.env` will never be committed:

```sh
git check-ignore -q .env && echo ".env is ignored"
```

If it is not ignored, stop and tell the builder to ask their squad lead.

## claim

Assign an issue to the builder. Look at it first:

```sh
gh issue view <number> --json number,title,state,assignees,labels
```

- Someone else is assigned: stop. The builder sorts it out with the squad lead. Never remove another person.
- Closed: stop and say so.

Then:

```sh
gh issue edit <number> --add-assignee @me
```

Check the builder is now the only assignee. If the issue has no size label or no guild label, tell the builder: work can start, but the lead adds both before it earns points.

## start

Make a branch for one issue from a fresh `main`. If there are uncommitted changes, stop and ask what to do with them first.

```sh
git switch main
git pull --ff-only origin main
git switch -c issue-<number>-<short-slug>
```

The slug comes from the issue title: lowercase, letters, digits and hyphens, three to five words, 40 characters at most. Issue 12, "Add wallet connect button", becomes `issue-12-add-wallet-connect-button`.

If `git pull --ff-only` fails, local `main` has commits of its own. Stop and explain; never push them.

## sync

Bring the branch up to date with `main`. Commit or stash uncommitted work first, after asking.

```sh
git fetch origin
git rebase origin/main
```

On a conflict, stop:

1. Run `git status` and list the files in conflict.
2. For each one, explain in plain words what `main` changed and what this branch changed.
3. Propose a resolution and wait for the builder to agree.
4. Apply it, then `git add <file>` and `git -c core.editor=true rebase --continue`. The editor setting keeps each commit message as it is instead of opening an editor.

To back out instead: `git rebase --abort`.

If the branch was already pushed, a rebase needs `git push --force-with-lease` to update it. Say so before running it.

## ship

1. Run the checks the repo defines. With no `package.json` there are none, so skip this step. Otherwise each is skipped when its script does not exist:

   ```sh
   npm run lint --if-present
   npm run typecheck --if-present
   npm test --if-present
   ```

   Report each result. Fix failures first, or tell the builder what failed.
2. Run `git status`. Never stage `.env` or anything that holds a secret.
3. Push the branch:

   ```sh
   git push -u origin HEAD
   ```

4. Open the pull request from the repo's template. Read `.github/pull_request_template.md`, fill in each section from the work done, put `Closes #<number>` on the linked issue line, and delete the template's `<!-- -->` hint lines. HackingDAO reads the body as written, hidden comments included, so a closing word (close, fix, resolve and their forms) must not stand before any other issue number. Pass the body on standard input:

   ```sh
   gh pr create --base main --title "<issue title>" --body-file -
   ```

5. Check the link took:

   ```sh
   gh pr view --json body --jq .body
   gh pr view --json closingIssuesReferences --jq '.closingIssuesReferences[].number'
   ```

   The body must contain `Closes #<number>` and the second command must print that number. If not, fix the body with `gh pr edit --body-file -`.
6. Tell the builder the lead reviews and merges. The builder does not merge their own pull request.

## after-merge

Only after the pull request is merged:

```sh
gh pr view <branch> --json state --jq .state
```

It must print `MERGED`. Anything else: stop. Then:

```sh
git switch main
git pull --ff-only origin main
git branch -d issue-<number>-<short-slug>
```

If `git branch -d` refuses because the pull request was squashed or rebased, and the state above is `MERGED`, use `git branch -D` for that branch only. If the branch is still on GitHub:

```sh
git push origin --delete issue-<number>-<short-slug>
```
