# Exercise 4 (Optional): Deploy to Azure App Service

**Goal:** Publish the application to Azure App Service by storing the web app's publish profile as a GitHub secret and letting the workflow deploy with it.

**Time:** 15-20 minutes

Prerequisite: Exercises 1-3 are complete, and you have the Azure and GitHub access listed under Prerequisites in [Exercise 0](exercise-0-introduction.md#prerequisites). The `Deploy to Azure App Service` job in `.github/workflows/ci.yml` runs only on a push to `master` after `Build & Test` passes, and is skipped until the repository variable `AZURE_APP_NAME` is set.

## Settings at a glance

| Where | Setting | Value |
|-------|---------|-------|
| Azure web app | Runtime stack / OS | Python 3.12 / Linux |
| Azure web app → **Environment variables → App settings** | `SCM_DO_BUILD_DURING_DEPLOYMENT` | `true` |
| Azure web app → **Configuration → Stack settings** | **Startup Command** | `gunicorn --chdir app/backend --bind=0.0.0.0:8000 --workers 1 --timeout 600 app:app` |
| Azure web app → **Configuration → General settings** | **SCM Basic Auth Publishing Credentials** | On |
| GitHub → **Settings → Environments → `production`** → environment secret | `AZURE_WEBAPP_PUBLISH_PROFILE` | Full contents of the downloaded publish profile |
| GitHub → **Settings → Secrets and variables → Actions → Variables** | `AZURE_APP_NAME` | Your web app name |

## Part A: Create the web app

In the Azure portal, select **Create a resource → Web App** and use:

| Setting | Value |
|---------|-------|
| Name | A unique name, for example `aidlc-app` (this is `AZURE_APP_NAME`) |
| Publish | Code |
| Runtime stack | Python 3.12 |
| Operating System | Linux |
| Region | Your nearest region |
| Pricing plan | Basic B1 recommended; Free F1 works but is slow |

On the **Deployment** tab, leave GitHub Actions continuous deployment disabled. Skip the database. Select **Review + create**, then **Create**.

## Part B: Configure the web app

1. Open the web app, then **Settings → Environment variables → App settings → + Add**. Add `SCM_DO_BUILD_DURING_DEPLOYMENT` with value `true`, select **Apply**, then **Apply** again at the bottom of the page and **Confirm**. This makes Azure install the packages from `requirements.txt`.
2. Open **Settings → Configuration → Stack settings** and set **Startup Command** to:

   ```text
   gunicorn --chdir app/backend --bind=0.0.0.0:8000 --workers 1 --timeout 600 app:app
   ```

3. Open **Settings → Configuration → General settings**. Set **SCM Basic Auth Publishing Credentials** to **On**. Without this, the publish profile cannot be used, and **Download publish profile** may be unavailable or the deployment fails with 401 or 403.
4. Select **Save**, and confirm the change when prompted. The web app restarts.

## Part C: Download the publish profile

Do this after Part B, so the profile is generated with basic authentication enabled. On the web app **Overview** page, select **Download publish profile**. Open the downloaded `.PublishSettings` file in a text editor, then select and copy everything from the opening `<publishData>` tag to the closing `</publishData>` tag, unedited.

Treat the file like a password. Do not commit it or share it, and delete it after the next part. If it leaks, use **Reset publish profile** on the Overview page.

## Part D: Add the GitHub settings

1. In your repository, open **Settings → Environments → New environment**, name it exactly `production`, and select **Configure environment**.
2. In that environment, under **Environment secrets**, select **Add environment secret**:
   - **Name:** `AZURE_WEBAPP_PUBLISH_PROFILE` (must match exactly, including case)
   - **Secret:** paste the full publish profile contents you copied
3. Open **Settings → Secrets and variables → Actions → Variables → New repository variable**:
   - **Name:** `AZURE_APP_NAME`
   - **Value:** the web app name from Part A, for example `aidlc-app`

The workflow reads these two values in the deploy step:

```yaml
app-name: ${{ vars.AZURE_APP_NAME }}
publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
```

## Part E: Deploy and verify

1. Push or merge a change to `master`, or open **Actions** and re-run the latest `master` run.
2. Wait for `Build & Test`, then watch **Deploy to Azure App Service**. The first deployment takes a few minutes while Azure installs the dependencies.
3. Open the web app's **Default domain** and verify:
   - `https://<default-domain>/` shows the task manager.
   - `https://<default-domain>/api/tasks` returns the API response.
   - `https://<default-domain>/docs` shows Swagger UI.

Troubleshooting:

- **401 or 403 on deploy:** Confirm **SCM Basic Auth Publishing Credentials** is **On**, and that the secret holds the complete, unedited profile. If you used **Reset publish profile**, download the profile again and update the secret.
- **"Application Error" page:** Check **Monitoring → Log stream**. Confirm `SCM_DO_BUILD_DURING_DEPLOYMENT=true` and the startup command.
- **Deploy job skipped:** `AZURE_APP_NAME` is not set, or the run was not on `master`.
- **Deploy job cannot find the secret:** The secret name or the `production` environment name does not match exactly.

Keep one worker and one instance. Tasks are held in memory and reset on restart.

## Completion check

- [ ] Web app created with Python 3.12 on Linux, with the build setting, startup command, and basic auth publishing enabled.
- [ ] Publish profile stored as the `AZURE_WEBAPP_PUBLISH_PROFILE` secret in the `production` environment, and `AZURE_APP_NAME` variable added.
- [ ] The deploy job succeeded, and the UI, `/api/tasks`, and `/docs` work on the Azure hostname.
- [ ] The publish profile file was deleted, and nothing was committed to the repository.
