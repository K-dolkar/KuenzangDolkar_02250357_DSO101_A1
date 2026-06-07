# Assignment 3 – CI/CD with GitHub Actions, Docker, and Render

**Student:** Kuenzang Dolkar | **ID:** 02250357 | **Course:** DSO101

## GitHub Repository

[https://github.com/KuenzangDolkar/todo-app](https://github.com/KuenzangDolkar/todo-app)

## Live Deployment

[https://todo-app-XXXX.onrender.com](https://todo-app-XXXX.onrender.com)
*(Replace with actual Render URL after deployment)*

---

## Steps Taken

### 1. Repository Setup
- Used the Node.js To-Do List REST API from Assignment 1 as the base application.
- Confirmed `package.json` contains the required scripts: `start`, `build`, `test`, and `test:local`.
- Ensured the GitHub repository is set to **public**.

### 2. Dockerfile
Created a `Dockerfile` following the assignment's expected structure using `node:20-alpine` as the base image. The file installs dependencies, runs tests at build time, exposes port 3000, and starts the app with `npm start`.

### 3. GitHub Actions Workflow (`.github/workflows/deploy.yml`)
The workflow triggers on every push to the `main` branch and runs the following steps:
1. **Checkout** the repository.
2. **Set up Node.js 20** and install dependencies.
3. **Run tests** using Jest to gate the build.
4. **Login to DockerHub** using repository secrets.
5. **Build and push** the Docker image to DockerHub as `<username>/todo-app:latest`.
6. **Trigger a Render deploy** via the Render deploy hook webhook URL.

### 4. GitHub Secrets Added
The following secrets were added under **Settings → Secrets and variables → Actions**:

| Secret | Description |
|---|---|
| `DOCKERHUB_USERNAME` | DockerHub account username |
| `DOCKERHUB_TOKEN` | DockerHub access token (not password) |
| `RENDER_DEPLOY_HOOK_URL` | Render deploy hook URL from the service dashboard |

> **Note:** No credentials are hardcoded in any file. All sensitive values are referenced via `${{ secrets.* }}`.

### 5. Render.com Setup
1. Created a new **Web Service** on Render.com.
2. Selected **"Deploy an existing image from a registry"**.
3. Entered the DockerHub image URL: `docker.io/<username>/todo-app:latest`.
4. Set the environment variable `PORT=3000`.
5. Copied the **Deploy Hook URL** from the service settings and added it as the `RENDER_DEPLOY_HOOK_URL` GitHub secret.

---

## Screenshots

### GitHub Actions – Successful Workflow
![GitHub Actions Workflow](screenshots/github-actions-success.png)

### DockerHub – Image Pushed
![DockerHub Image](screenshots/dockerhub-image.png)

### Render.com – Live Deployment
![Render Deployment](screenshots/render-deployment.png)

---

## Challenges Faced

- **Render does not auto-redeploy on new DockerHub pushes.** Render only watches its own registry or a connected GitHub repo. To automate redeployment after a DockerHub push, a deploy hook webhook must be called explicitly from the GitHub Actions workflow using `curl`.
- **Docker build runs tests**, which means the build fails if tests fail — this is intentional and enforces quality gating, but required `npm install` (not `--production`) so devDependencies like Jest are available.
- **Secret management**: Keeping credentials out of the codebase required careful use of GitHub Secrets for all three services (DockerHub username, DockerHub token, Render webhook).

---

## Learning Outcomes

- Understood the end-to-end CI/CD pipeline: code push → automated test → Docker build → registry push → cloud redeploy.
- Learned how GitHub Actions workflows are structured: triggers, jobs, steps, and the use of community actions (`actions/checkout`, `docker/login-action`).
- Gained practical experience containerising a Node.js app and managing a public DockerHub repository.
- Understood how Render.com deploys from a container image and how to automate redeployment using webhooks.
- Reinforced the importance of secrets management — never hardcoding credentials in source code.
