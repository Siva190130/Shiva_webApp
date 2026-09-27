# My first Azure web app

A small Node.js website for practicing deployment to **Azure App Service**. It includes a welcome page, an interactive button, and a `/health` endpoint. No database, secrets, or external packages are required.

## Run locally

Install Node.js 24 LTS, open a terminal in this folder, and run:

```sh
npm start
```

Open http://localhost:3000. Stop the server with Ctrl+C.

## Deploy using the Azure portal

This walkthrough uses GitHub as the source and the Azure portal to configure deployment. You need an Azure subscription and a GitHub account.

1. Create a GitHub repository. Upload `package.json`, `package-lock.json`, `server.js`, and the `public` folder to the repository root. You can use GitHub's **Add file > Upload files** page. Keep `public/index.html` inside the `public` folder.
2. Open https://portal.azure.com and search for **App Services**. Select **Create > Web App**.
3. On the Basics tab, select your subscription and create a resource group such as `rg-webapp-practice`.
4. Enter a globally unique app name, such as `shiva-practice-12345`.
5. Set **Publish** to **Code**, **Runtime stack** to **Node 24 LTS**, and **Operating System** to **Linux**. If Node 24 is unavailable in your region, choose a newer supported Node LTS runtime.
6. Choose a region and an App Service plan. Select **Free F1** if available for your configuration. Other tiers can incur charges; check the displayed price before creating the app.
7. Select **Review + create**, then **Create**. When provisioning finishes, select **Go to resource**.
8. Open **Deployment Center** in the app's menu. Set the source to **GitHub**, authorize access, and select your repository and branch (usually `main`). Choose **GitHub Actions** as the build provider if prompted. Follow the authentication prompts and select **Save**. Azure generates a deployment workflow in your repository.
9. Open your GitHub repository's **Actions** tab. Wait until the deployment workflow succeeds. The app needs no build command; its start command is `npm start`.
10. In the Azure app's **Overview** page, select **Browse** or open its **Default domain**. You should see the welcome page. Append `/health` to the URL to see `{"status":"healthy"}`.

Portal labels may vary slightly. If asked for a startup command, use `npm start` under **Settings > Configuration > General settings** (or the portal's **Startup Command** setting), save, and restart the app. The server already listens on Azure's `PORT` environment variable and all network interfaces.

## Practice deploying an update

Edit the heading in `public/index.html` on GitHub and commit to the connected branch. GitHub Actions deploys the update automatically. When it succeeds, refresh your Azure website.

## Troubleshooting

- **Deployment fails:** Open the failed run in GitHub Actions and inspect the failed step. Ensure the app files are at the repository root.
- **Application error:** Check **Monitoring > Log stream** in Azure. Enable application logging if prompted. Confirm the Node runtime and startup command match the settings above.
- **Old page appears:** Wait for the latest workflow to finish, then refresh your browser.

## Clean up

When finished, delete `rg-webapp-practice` through the Azure portal if it contains only this practice project. This removes the app and its plan. Stopping an app alone does not stop charges for a paid App Service plan.
