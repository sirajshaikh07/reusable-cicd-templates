# Reusable GitHub Actions CI/CD Templates

This repository contains reusable and highly customizable GitHub Actions workflow templates for Java/Spring Boot and Docker applications. 

You can copy and adapt these templates to quickly set up robust CI/CD pipelines in other projects.

## 📂 Included Templates

1. **`ci-test-template.yml`**: Continuous Integration template that compiles and runs unit/integration tests on Pull Requests (Maven).
2. **`docker-build-push-template.yml`**: Docker compilation, build, and push (multi-tagging with SHA + branch name) to Docker Hub.
3. **`deploy-test-template.yml`**: Automated SSH deployment to a TEST environment, including container orchestration, service startup, and automated smoke test checks.
4. **`deploy-prod-template.yml`**: Production deployment pipeline featuring manual confirmation gates, environment deployment checks, and safe auto-rollback on failure.

## 🚀 How to Use

1. Copy the desired template(s) to your project's `.github/workflows/` directory.
2. Rename the files (e.g., remove the `-template` suffix).
3. Update the environment variables (`env:`) at the top of each file (such as `WORKING_DIR`, `APP_PORT`, `CONTAINER_NAME`) to match your new application.
4. Add the required secrets to your GitHub repository settings under **Settings ➔ Secrets and variables ➔ Actions**.
