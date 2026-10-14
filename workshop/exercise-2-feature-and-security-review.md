# Exercise 2: Server-Side Pagination, Pull Request, and GitHub Advanced Security Review

**Goal:** Add server-side pagination, open a pull request, and review CodeQL and secret scanning results.

**Time:** 30-40 minutes

## Part A: Check the current app

```powershell
git switch master
git pull --ff-only
.\run.ps1
```

Open `http://localhost:5000` and note that all 15 tasks load in one list, with no pagination. Stop with Ctrl+C.

## Part B: Create a feature branch

```powershell
git switch -c feature/server-pagination
```

## Part C: Implement server-side pagination

Use this prompt in Copilot Chat:

> Add server-side pagination to `GET /api/tasks`. Accept a one-based `page` (default 1) and `page_size` (default 10, max 50), sort by ascending task ID, and return `{"items": [], "page": 1, "page_size": 10, "total": 15, "total_pages": 2}`. An empty result returns empty `items` and `total_pages: 0`. Return HTTP 400 with a JSON error for non-integer values, `page` below 1, or `page_size` outside 1-50. Update OpenAPI, the frontend table, and focused backend tests. Add accessible Previous/Next controls and a page indicator, disabled at the first and last page. The browser must request only the visible page from the API. Change only source and test files under `app/`, and do not change unrelated behavior.

After Copilot finishes, review and accept the code changes, then run the app and check the pagination feature:

```powershell
.\run.ps1
```

Open `http://localhost:5000` and confirm Previous/Next, the page indicator, and the disabled buttons at the first and last page work. Stop with Ctrl+C.

## Part D: Push and open a pull request

```powershell
git add app
git commit -m "Add server-side task pagination"
git push -u origin feature/server-pagination
```

Select **Compare & pull request**, target `master`, and summarize your changes. Creating the PR triggers **CodeQL** and **Dependency Review**; pushing the feature branch alone does not. Once the scans finish, open **Checks** to review code scanning alerts and newly introduced dependency vulnerabilities.

If you enabled automatic **Copilot code review** at the repository level in Exercise 1, review its suggestions on the PR and address relevant feedback. Copilot review does not replace the required human approval.

**Build & Test** runs on pushes to `master`, including merges.

**Leave the pull request open.** The `master` ruleset requires one approval, and Exercise 3 stacks a second PR on this branch.

## Part E: Review Advanced Security results

> **Note:** Workflow-based scan results may take a few minutes to appear. Wait for the CodeQL and Dependency Review workflows to finish, then refresh before reviewing the results.

Open **Security and quality** and review all available security findings, not just those introduced by this PR:

- **Code scanning:** code vulnerabilities detected by CodeQL or other configured tools.
- **Dependabot:** vulnerable dependencies.
- **Secret Protection / Secret scanning:** exposed secrets and push protection alerts.

For each finding, review its severity, affected code or dependency, and recommended fix. Review any other available security alerts and record your findings. If no alerts appear, record that result.

## Completion check

- [ ] Pagination is done by the API, tested, and works at `http://localhost:5000`.
- [ ] Pull request into `master` is open and its checks passed.
- [ ] All available security findings were reviewed and recorded, including code scanning, dependency, and Secret Protection alerts; no secrets were committed.

Continue to [Exercise 3](exercise-3-readme-update.md).