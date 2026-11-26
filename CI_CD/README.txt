FRONTEND CI/CD DOCUMENTATION
=============================

Location
--------
The GitHub Actions workflow definitions are stored in:
  .github/workflows/

This documentation is stored in:
  CI_CD/README.txt

1. .github/workflows/ci.yml — Frontend CI
-----------------------------------------
Runs automatically for pull requests targeting main, pushes to main, and
manual workflow dispatches.

What it does:
- Checks out the repository.
- Detects whether a React/Node project (package.json) exists.
- Sets up Node.js 22 and enables Corepack when a project is present.
- Installs dependencies using the detected pnpm, Yarn, or npm lockfile.
- Runs the lint script when the project defines one.
- Runs the test script when the project defines one.
- Runs the production build when a build script exists.
- Uploads the generated dist/ or build/ directory as a CI artifact.

The repository is currently a scaffold. The workflow intentionally warns and
skips application checks until package.json and the React application exist.
When the app is added, ensure package.json has appropriate lint, test, and
build scripts and commit the package manager lockfile.

2. .github/workflows/release.yml — Frontend release packaging
-------------------------------------------------------------
Runs when a version tag beginning with "v" is pushed, or when manually started.

What it does:
- Checks out the tagged source.
- Sets up Node.js and installs the frontend dependencies.
- Builds the production frontend bundle.
- Uploads dist/ or build/ as a versioned GitHub Actions artifact.

This workflow packages a release build. It does not publish the website to a
hosting provider yet. Production hosting and deployment credentials must be
selected and configured before automatic deployment is added.

3. Deployment notes
-------------------
The React hosting provider has not yet been selected. No production deployment
or preview deployment is configured. Once the hosting target is decided,
add a protected deployment environment and provider-specific deployment steps.
Do not store credentials in source files; use GitHub Actions secrets.
