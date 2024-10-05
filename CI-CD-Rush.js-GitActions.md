# GitHub Actions Configuration Guide for Rush.js

This document outlines the setup of GitHub Actions for a project using Rush.js, facilitating both development and production workflows.

## Workflow Trigger Setup

The workflow is designed to trigger on `push` and `pull_request` events to the `develop` branch for development, and `push` events to the `main` branch for production builds.

```
name: CI

on:
  push:
    branches: ["develop", "main"] # Triggers on push to develop and main branches
  pull_request:
    branches: ["develop"] # Triggers on pull requests to develop branch
```

## Jobs and Steps

The job `build` runs on an Ubuntu latest environment and consists of several steps outlined below:

### Checkout Code

```
- uses: actions/checkout@v3
  with:
    fetch-depth: 2
```

### Git Configuration

This step configures the user name and email for Git operations.

```
- name: Git config user
  uses: snow-actions/git-config-user@v1.0.0
  with:
    name: "Your Service Account Name"
    email: "Your email"
```

### Node.js Setup

Sets up Node.js environment using the specified version.

```
- uses: actions/setup-node@v3
  with:
    node-version: 16
```

### Verify Change Logs

Verifies change logs using Rush.

```
- name: Verify Change Logs
  run: node common/scripts/install-run-rush.js change --verify
```

### Dependency Installation

Installs dependencies using Rush.

```
- name: Rush Install
  run: node common/scripts/install-run-rush.js install
```

### Build Steps

Conditional build steps for development and production.

```
- name: Rush rebuild
  if: github.ref == 'refs/heads/develop'
  run: node common/scripts/install-run-rush.js rebuild --verbose

- name: Rush rebuild for production
  if: github.ref == 'refs/heads/main'
  run: node common/scripts/install-run-rush.js rebuild --verbose --production
```

## Explanation

- **Development Builds**: Triggered on pushes and pull requests to the `develop` branch. The build is less optimized, focusing on speed for continuous development.
- **Production Builds**: Triggered on pushes to the `main` branch. These builds are fully optimized for production deployment.

This setup ensures that development and production environments are managed efficiently, with tailored actions for each scenario.
