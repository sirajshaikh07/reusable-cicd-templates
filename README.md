# Reusable GitHub Actions CI/CD Templates

This repository contains reusable and highly customizable GitHub Actions workflow templates for Java/Spring Boot and Docker applications. 

You can copy and adapt these templates to quickly set up robust CI/CD pipelines in other projects.

## 📂 Included Templates

1. **`ci-test-template.yml`**: Continuous Integration template that compiles and runs unit/integration tests on Pull Requests (Maven).
2. **`docker-build-push-template.yml`**: Docker compilation, build, and push (multi-tagging with SHA + branch name) to Docker Hub.
3. **`deploy-test-template.yml`**: Automated SSH deployment to a TEST environment, including container orchestration, service startup, and automated smoke test checks.
4. **`deploy-prod-template.yml`**: Production deployment pipeline featuring manual confirmation gates, environment deployment checks, and safe auto-rollback on failure.

## 🚀 How to Use (Step-by-Step Setup)

Follow these steps to bring these templates into your new repository:

### Step 1: Clone the Templates Locally
Clone this repository to a temporary directory on your local machine:
```bash
git clone https://github.com/sirajshaikh07/reusable-cicd-templates.git
```

### Step 2: Copy Files to Your Project
Navigate to your new project's root directory:

1. **GitHub Workflows Setup:**
   * Create the `.github/workflows/` directory structure:
     ```bash
     mkdir -p .github/workflows
     ```
   * Copy the required workflow files into your project and rename them to remove the `-template` suffix:
     ```bash
     # Unix/Linux/macOS:
     cp path/to/templates/ci-test-template.yml .github/workflows/01-ci-test.yml
     cp path/to/templates/docker-build-push-template.yml .github/workflows/02-build-and-push-docker.yml
     cp path/to/templates/deploy-test-template.yml .github/workflows/03-deploy-test.yml
     cp path/to/templates/deploy-prod-template.yml .github/workflows/04-deploy-prod.yml
     ```

2. **Dockerfile Setup:**
   * Choose the Dockerfile that matches your technology stack from the `dockerfiles/` folder.
   * Copy it to the root of your new project and rename it to `Dockerfile`:
     ```bash
     # Example for Node.js:
     cp path/to/templates/dockerfiles/Dockerfile.node ./Dockerfile
     ```

3. **.dockerignore Setup:**
   * Copy the `.dockerignore-template` to your project's root and rename it to `.dockerignore`:
     ```bash
     cp path/to/templates/.dockerignore-template ./.dockerignore
     ```

### Step 3: Configure Variables and Secrets
1. **Customize Environment Variables:** Open the `.yml` files in your project and update the variables in the `env:` block (like `WORKING_DIR`, `APP_PORT`, `CONTAINER_NAME`).
2. **GitHub Secrets & Environments:** Refer to [AI_INSTRUCTIONS.md](AI_INSTRUCTIONS.md) for the complete list of repository secrets, environments (`test`, `production`), and settings required on GitHub.
