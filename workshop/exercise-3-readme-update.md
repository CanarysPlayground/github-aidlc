# Exercise 3: Stacked PR — Update the README with Copilot

**Goal:** Document pagination in `README.md`, open a stacked PR, and merge both PRs.

**Time:** 20-25 minutes

## Part A: Create a documentation branch

Keep the pagination PR open and branch from `feature/server-pagination`:

```powershell
git switch feature/server-pagination
git pull --ff-only
git switch -c docs/readme-pagination
```

## Part B: Document pagination

Use this prompt in Copilot Chat:

> Update only `README.md` to document task pagination based on the current code. Cover `GET /api/tasks` parameters, defaults and limits, response fields, an example request and response, validation errors, and Previous/Next controls. Update existing descriptions for consistency. Keep it concise and preserve unrelated content.

Review and accept the changes. Check accuracy against the app and confirm `git diff --stat` lists only `README.md`.

## Part C: Commit and open a stacked pull request

```powershell
git add README.md
git commit -m "Document task pagination in README"
git push -u origin docs/readme-pagination
```

Create a PR from `docs/readme-pagination` into **`feature/server-pagination`**, not `master`. This stacks the documentation on the pagination PR. Select **Create stack** or accept the stack banner if offered. Confirm **Files changed** lists only `README.md`.

CodeQL and Dependency Review do not run while this PR targets the feature branch. CI runs on pushes to `master`.

## Part D: Approve and merge the stack

1. Ask a facilitator or teammate with write access to approve each PR. Self-approval and Copilot review do not satisfy the required human approval.
2. Merge the pagination PR first. Confirm the README PR now targets `master` (change its base if needed), review its checks, then merge it.
3. Confirm `CI/CD` and `CodeQL` succeed in **Actions**, then update local `master` and check the README:

```powershell
git switch master
git pull --ff-only
```

## Completion check

- [ ] Only `README.md` changed in the documentation PR, accurately describing pagination.
- [ ] Both PRs received human approval and merged bottom up; CI and CodeQL succeeded.

Optional: continue to [Exercise 4](exercise-4-deploy-to-azure.md) to deploy the application to Azure App Service.
