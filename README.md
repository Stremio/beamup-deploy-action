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
- `USERNAME_GITHUB`: Your GitHub username (required)
- `PROJECT_NAME`: The name of your project on Beamup (required)

## Usage

This workflow can be triggered manually (via the GitHub Actions UI) or automatically when a new release is published.

See [`action.yml`](./action.yml) for the full workflow logic.

## License

MIT