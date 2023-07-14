# GitHub Action: Helm Chart Repo

Turn your GitHub repo with charts into a self-hosted Helm charts repo using GitHub Container Registry, ghcr.io (OCI-based).

This is an alternative to the action by helm called 'Helm Chart Releaser', with the main difference being that they are utilizing GitHub Pages as the charts repo. 

This GitHub action gives you the following benefits:
- Helm natively supports OCI based registries since helm v3.8.0, using ghcr.io makes way more sense now instead of using GitHub Pages as a workaround.
- Works on private repos (without paying for Enterprise plan).

See live example at: https://github.com/JimCronqvist/helm-charts


Example Workflow

Create a workflow (eg: `.github/workflows/helm.yml`):

```
name: Helm Publish

on:
  push:
    branches: [ 'master', 'main' ]
  workflow_dispatch:
  
jobs:
  helm:
    permissions:
      contents: read
      packages: write
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Install Helm
        uses: azure/setup-helm@v3
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
      
      - name: Build and Push the Helm Charts to GitHub Container Registry
        uses: JimCronqvist/action-helm-chart-repo@master
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
```