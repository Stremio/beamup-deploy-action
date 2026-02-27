# Beamup Deploy Action

Deploy Stremio addons to a Beamup server using GitHub Actions.

## Overview
This action automates the deployment of your Stremio addon project to a Beamup server. It is designed to be used in your CI/CD pipeline and supports configuration via secrets and environment variables.

## Features
- Validates required secrets
- Sets up SSH securely (supports ssh-agent)
- Syncs GitHub keys with Beamup
- Pushes your code to the Beamup server using Dokku
- Cleans up sensitive files after deployment

## Prerequisites

In order to deploy you will need:

 - a valid Beamup host
 - a GitHub account
 - a dedicated SSH key added to your GitHub account

## Inputs & Secrets

The following secrets must be set in your repository:

- `SSH_PRIVATE_KEY`: The private SSH key for deployment (required)
- `BEAMUP_HOST`: The Beamup server hostname (required)
- `PROJECT_NAME`: The name of your project on Beamup (required)
- `USERNAME_GITHUB`: Your GitHub username (required)

## Usage

This workflow can be triggered manually (via the GitHub Actions UI) or automatically when a new release is published.

```yaml
name: Deploy to Beamup
run-name: Deploy to Beamup by @${{ github.actor }}

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Deploy
        uses: Stremio/beamup-deploy-action@v1.0.1
        with:
          ssh_private_key: ${{ secrets.SSH_PRIVATE_KEY }}
          beamup_host: ${{ secrets.BEAMUP_HOST }}
          username_github: ${{ secrets.USERNAME_GITHUB }}
          project_name: ${{ secrets.PROJECT_NAME }}
```

You can map inputs from either `secrets` or `env` values. For example:

```yaml
env:
  SSH_PRIVATE_KEY: ${{ secrets.SSH_PRIVATE_KEY }}
  BEAMUP_HOST: ${{ vars.BEAMUP_HOST }}
  USERNAME_GITHUB: ${{ vars.USERNAME_GITHUB }}
  PROJECT_NAME: ${{ vars.PROJECT_NAME }}

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Deploy
        uses: Stremio/beamup-deploy-action@v1.0.1
        with:
          ssh_private_key: ${{ secrets.SSH_PRIVATE_KEY }}
          beamup_host: ${{ env.BEAMUP_HOST }}
          username_github: ${{ env.USERNAME_GITHUB }}
          project_name: ${{ env.PROJECT_NAME }}
```
NOTE: `SSH_PRIVATE_KEY` should be always used in `secrets`

For a ready-to-use workflow file, see [`example.yml`](./example.yml).

See [`action.yml`](./action.yml) for the complete input/output contract.

## Security assumptions

This action intentionally relies on the following assumptions:

- `sync_key_base32` default is globally distributed and is not treated as a secret.
- Beamup host keys are bootstrapped with `ssh-keyscan` (no fingerprint pinning in this action).
- Deployment always force-pushes `HEAD:master`.

## License

MIT
