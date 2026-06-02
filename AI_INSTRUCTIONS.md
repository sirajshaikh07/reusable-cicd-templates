# AI Integration Instructions: How to Apply this CI/CD Flow to a New Project

This document provides step-by-step instructions for an AI coding assistant (e.g., Gemini, Claude, GPT) to successfully integrate and configure these CI/CD templates into any new application/project.

---

## 🎯 Target Pipeline Architecture
The pipeline consists of 4 decoupled workflows:
1. **CI Build & Test (`ci-test-template.yml`)**: Runs on Pull Requests to `develop` and `main` branches. Validates compile and runs tests.
2. **Docker Build & Push (`docker-build-push-template.yml`)**: Runs automatically on direct pushes or merged PRs to the `develop` branch. Pushes image to Docker Hub with short-SHA and branch-name tags.
3. **TEST Deployment (`deploy-test-template.yml`)**: Automatically triggers via `workflow_run` after the Docker Build workflow completes on the `develop` branch. SSHs to the test VM, deploys the container, and runs a smoke check.
4. **PROD Deployment (`deploy-prod-template.yml`)**: Triggered manually via `workflow_dispatch` on GitHub. Requires manual confirmation input (`DEPLOY-PROD`) and required reviewer approval. Features safe auto-rollback on container startup failure.

---

## 🤖 AI Instructions for Integration

If you are an AI tasked with setting up this pipeline in a new repository, follow these steps:

### Step 1: Analyze the Codebase
Before editing files, run commands to understand:
* **Build Tool:** Is it Maven (`pom.xml`, `./mvnw`) or Gradle (`build.gradle`, `./gradlew`)?
* **Java Version:** Check the Java compiler configuration in the build file (e.g., `<java.version>21</java.version>`).
* **Main Application Properties:** Look at `application.properties`/`application.yml` to find:
  * Application server port (usually `8080`).
  * Database requirements (e.g., PostgreSQL, MySQL, H2).
  * Spring Profiles (e.g., active profiles for `test` or `local`).

### Step 2: Copy and Rename Workflow Files
Copy the templates from this directory into the new project's `.github/workflows/` directory and rename them:
* `ci-test-template.yml` ➔ `.github/workflows/01-ci-test.yml`
* `docker-build-push-template.yml` ➔ `.github/workflows/02-build-and-push-docker.yml`
* `deploy-test-template.yml` ➔ `.github/workflows/03-deploy-test.yml`
* `deploy-prod-template.yml` ➔ `.github/workflows/04-deploy-prod.yml`

### Step 3: Customize Workflow Configurations
Inside the copied `.yml` files, search and update the following settings under the `env:` block or top-level configurations:

1. **`WORKING_DIR`**: Set this to `.` if the project is in the root directory. If it is in a subdirectory (e.g., a monorepo with `./backend`), change it to `./backend`.
2. **`JAVA_VERSION` & `JAVA_DISTRIBUTION`**: Match the version found in the new project.
3. **`APP_PORT` & `CONTAINER_PORT`**: Update these to match the port the application listens on (default is `8080`).
4. **`CONTAINER_NAME`**: Choose a unique container name for this application (e.g., `my-new-app-test`).
5. **Database Configuration**: If the new application does not use PostgreSQL (e.g., MySQL or H2), modify the `services:` block in the CI/Build files and the database driver variables in the deploy script.

### Step 4: Configure GitHub Repository Settings
Guide the human developer to configure the following settings on GitHub.com:

#### 1. Set the Default Branch to `develop`
Since `workflow_run` (in `03-deploy-test.yml`) evaluates triggers using the repository's default branch, the default branch **must be set to `develop`** on GitHub (**Settings ➔ General ➔ Default Branch**) to prevent environment protection rule failures.

#### 2. Create GitHub Environments
Go to **Settings ➔ Environments** and create two environments:
1. **`test`**
2. **`production`**

*For `production`, check the **Required reviewers** box and add the developer's username.*

#### 3. Add Secrets

**Repository-Level Secrets** (Settings ➔ Secrets and variables ➔ Actions):
* `DOCKERHUB_USERNAME`: Docker Hub username.
* `DOCKERHUB_TOKEN`: Docker Hub Personal Access Token.
* `DOCKERHUB_REPO`: Repository name (e.g., `username/new-app-repo`).
* `TEST_DB_NAME`, `TEST_DB_USER`, `TEST_DB_PASSWORD` (used for test container orchestration in CI).

**Environment-Level Secrets** (Add individually inside `test` and `production` environments):
* `SERVER_HOST`: IP/domain of the target VM.
* `SERVER_USER`: SSH login username (e.g., `ubuntu`).
* `SERVER_SSH_KEY`: Private SSH Key (must have permissions to deploy on the target VM).
* `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`: Active database connection parameters used by the running container.

---

## ⚡ Execution Verification Checklists
After setting up the files and secrets, run these checks to verify integration:
- [ ] Push changes to `develop` to trigger compile, test, image build, and auto-deploy to the test VM.
- [ ] Open `http://<SERVER_HOST>:<PORT>/actuator/health` in a browser to verify test environment deployment.
- [ ] Trigger the Production deployment manually from the Actions tab using the `workflow_dispatch` button and verify confirmation security.
