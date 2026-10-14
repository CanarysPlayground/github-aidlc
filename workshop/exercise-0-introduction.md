# Exercise 0: Introduction to the Secure Task Manager Workshop

Complete the exercises in order. Each one has a hands-on task, commands, a Copilot prompt where relevant, and a completion check. For technical details about the application, see the project [README](../README.md).

## Exercises

1. [Exercise 1: Create your repository from the template and enable GitHub Advanced Security](exercise-1-repository-and-security.md)
2. [Exercise 2: Implement server-side pagination, open a pull request, and inspect GitHub Advanced Security findings](exercise-2-feature-and-security-review.md)
3. [Exercise 3: Stacked PR — update the README with Copilot and merge the stack](exercise-3-readme-update.md)
4. [Exercise 4 (optional): Deploy to Azure App Service](exercise-4-deploy-to-azure.md)

## Prerequisites

| Requirement | Access needed |
|-------------|---------------|
| GitHub account and repository | Permission to create a repository from the template and change its settings |
| Local tools | Git, PowerShell, and Python 3.10+ |
| GitHub Advanced Security | Free on a **public** repository, which you create in Exercise 1; repository admin access to turn the features on |
| GitHub Copilot | A Copilot license with access to agent mode and Copilot code review, used for the feature implementation, code review |
| Azure (optional, Exercise 4) | An Azure subscription and a resource group where you can create a Linux Web App (Contributor is enough) |

If a setting is locked or unavailable, ask your organization owner or the facilitator.

The template contains the CI and CodeQL workflows. The workshop creates a personal repository from the template, so participants do not push branches or exercise commits to the shared template repository.

## Completion checklist

- [ ] Personal repository created from the template and cloned locally, with `origin` pointing to that repository.
- [ ] CodeQL code scanning and the available GitHub Advanced Security features (secret scanning, push protection, Dependabot) enabled.
- [ ] First feature (pagination) pushed in a pull request and checked by GitHub Advanced Security and CI.
- [ ] Second change (README pagination details) opened as a stacked PR on the pagination PR, and the stack merged from the bottom up.
- [ ] (Optional) Application deployed to Azure App Service.

Continue to [Exercise 1](exercise-1-repository-and-security.md).
