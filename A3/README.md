# Assignment III – CI/CD Pipeline using GitHub Actions, DockerHub, and Render

## Project Overview

This project implements a Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Node.js To-Do List REST API application. The pipeline automates the process of building, testing, containerizing, and deploying the application whenever changes are pushed to the GitHub repository.

### Technologies Used

* GitHub – Source code hosting
* GitHub Actions – CI/CD automation
* Node.js & npm – Application runtime and package management
* Jest – Unit testing
* Docker – Containerization
* DockerHub – Container registry
* Render.com – Cloud deployment

---

## Steps Taken

### 1. Repository Setup

The To-Do List REST API from Assignment 1 was used as the base application. The repository was configured as public and the `package.json` file was verified to contain the required scripts for starting, building, and testing the application.

### 2. Docker Configuration

A Dockerfile was created using the official `node:20-alpine` image.

The Dockerfile performs the following tasks:

* Sets up the application environment
* Installs dependencies
* Copies source code
* Runs automated tests
* Exposes port 3000
* Starts the application using `npm start`

The application was tested locally using Docker to ensure it worked correctly before deployment.

### 3. GitHub Actions Workflow

A workflow file was created in:

```text id="ntz6vq"
.github/workflows/deploy.yml
```

The workflow automatically runs whenever code is pushed to the `main` branch.

The workflow performs the following steps:

1. Checks out the source code.
2. Logs in to DockerHub using GitHub Secrets.
3. Builds a Docker image.
4. Pushes the Docker image to DockerHub.
5. Triggers deployment on Render using a Deploy Hook URL.

This ensures every code update is automatically packaged and deployed.

### 4. GitHub Secrets

To keep credentials secure, the following GitHub Secrets were configured:

| Secret                 | Purpose                   |
| ---------------------- | ------------------------- |
| DOCKERHUB_USERNAME     | DockerHub username        |
| DOCKERHUB_TOKEN        | DockerHub access token    |
| RENDER_DEPLOY_HOOK_URL | Render deployment webhook |

No credentials were hardcoded in the repository.

### 5. Render Deployment

A new Web Service was created on Render.com using the Docker image stored on DockerHub.

Configuration steps included:

* Selecting "Deploy an Existing Image"
* Providing the DockerHub image URL
* Setting required environment variables
* Generating a Deploy Hook URL
* Adding the Deploy Hook URL as a GitHub Secret

Whenever a new image is pushed to DockerHub, GitHub Actions triggers a new deployment on Render.

---

## CI/CD Workflow

The deployment process follows the sequence below:

```text id="r2d7ig"
Code Push → GitHub Actions → Docker Build → DockerHub Push → Render Deployment → Live Application
```

This fully automates the build and deployment process.

---

## Challenges Faced

### Render Redeployment

Render does not automatically redeploy applications when DockerHub images are updated.

**Solution:** A Render Deploy Hook was configured and triggered from GitHub Actions after every successful Docker image push.

### DockerHub Authentication

Authentication issues occurred when pushing Docker images.

**Solution:** A DockerHub Access Token was generated and stored securely in GitHub Secrets.

### Credential Security

Managing credentials securely without exposing them in source code was important.

**Solution:** All sensitive values were stored in GitHub Secrets and referenced in the workflow.

---

## Learning Outcomes

Through this assignment, I learned:

* How CI/CD pipelines automate software delivery.
* How GitHub Actions workflows are configured and executed.
* How Docker containerizes applications.
* How DockerHub stores and distributes container images.
* How Render deploys containerized applications.
* How deployment webhooks automate cloud deployments.
* The importance of secure credential management using GitHub Secrets.
* Practical DevOps concepts used in modern software development.

---

## Conclusion

This assignment successfully implemented an automated CI/CD pipeline using GitHub Actions, DockerHub, Docker, and Render.com. The pipeline automatically builds, tests, packages, and deploys the Node.js application, reducing manual effort and improving deployment reliability while following modern DevOps best practices.
