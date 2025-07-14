# 🚀 Dokploy Github Action

This GitHub Action triggers a deployment in [Dokploy](https://dokploy.com) by updating the Docker image for a specific application.

Useful for automating deployments as part of your CI/CD pipeline.

---

## 📦 Features

- 🔒 Securely authenticates via API token
- 📦 Updates a Dokploy application with a new Docker image
- ⚙️ Easily integrates into your CI workflows

---

## 🛠️ Usage

Add this step to your GitHub Actions workflow:

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to Dokploy
        uses: tacticlaunch/dokploy-github-action@0.1.0
        with:
          token: ${{ secrets.DOKPLOY_TOKEN }} # Generate in Dokploy settings
          application_id: ${{ secrets.DOKPLOY_APP_ID }} # From /api/project.all
          image: ${{ steps.meta.outputs.tags }} # e.g. ghcr.io/username/app:latest
          base_url: ${{ secrets.DOKPLOY_URL }} # e.g. https://my-dokploy-instance.com
