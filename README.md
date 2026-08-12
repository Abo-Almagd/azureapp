# Task Manager

A simple full-stack task manager: Node.js + Express backend, vanilla HTML/CSS/JS frontend, tasks persisted to a local JSON file. Ready to deploy to Azure App Service.

## Run locally

```bash
npm install
npm start
```

Then open http://localhost:8080

## Deploy to Azure App Service

You have a few options — pick whichever fits your workflow.

### Option A: Azure CLI (fastest, no portal clicking)

```bash
# 1. Log in
az login

# 2. Create a resource group (skip if you already have one)
az group create --name taskapp-rg --location eastus

# 3. Create an App Service plan (Linux, free/basic tier)
az appservice plan create --name taskapp-plan --resource-group taskapp-rg --sku B1 --is-linux

# 4. Create the web app (Node 22 LTS runtime)
az webapp create --resource-group taskapp-rg --plan taskapp-plan --name <your-unique-app-name> --runtime "NODE:22-lts"

# 5. Deploy your code (run from inside the taskapp folder)
az webapp up --name <your-unique-app-name> --resource-group taskapp-rg --runtime "NODE:22-lts"
```

Replace `<your-unique-app-name>` with a globally unique name — this becomes `https://<your-unique-app-name>.azurewebsites.net`.

### Option B: VS Code Azure App Service extension

1. Install the "Azure App Service" extension in VS Code.
2. Sign in to Azure from the extension sidebar.
3. Right-click the `taskapp` folder → **Deploy to Web App**.
4. Choose "Create New Web App", pick Node 22 LTS as the runtime.

### Option C: GitHub Actions (CI/CD)

1. Push this folder to a GitHub repo.
2. In the Azure Portal, create a Web App (Node 22, Linux).
3. In the Web App's **Deployment Center**, connect it to your GitHub repo — Azure will auto-generate a GitHub Actions workflow that builds and deploys on every push to `main`.

## Notes

- The app listens on `process.env.PORT`, which Azure App Service sets automatically — no config changes needed.
- Task data is stored in `data/tasks.json`. On Azure's free/basic tiers this file persists across restarts but **not** across deployments/redeploys or scale-outs to multiple instances. For production use, swap this for a real database (Azure Cosmos DB, Azure SQL, or Azure Table Storage) — ask me and I can wire one in.
- A `/health` endpoint is included, useful for Azure health checks / Application Insights availability tests.
