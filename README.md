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
name: Build Docker images

on:
  push:
    tags:
      - 'v*'

jobs:
  build-and-push-dockerfile-image:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write

    steps:
      - name: Checkout repository
        uses: actions/checkout@v3

      - name: Log in to GitHub Container Registry
        uses: docker/login-action@v2
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and push Docker image
        uses: docker/build-push-action@v4
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: |
            ghcr.io/${{ github.repository }}:latest
            ghcr.io/${{ github.repository }}:${{ github.sha }}
          platforms: linux/amd64

      - name: Deploy to Dokploy
        uses: tacticlaunch/dokploy-github-action@0.1.0
        with:
          token: ${{ secrets.DOKPLOY_TOKEN }} # Generate in Dokploy settings (Settings -> Profile)
          application_id: ${{ secrets.DOKPLOY_APP_ID }} # From /api/project.all or last id in the application url (`https://DOKPLOY_URL/dashboard/project/jsY-g3aa0j3CjZJtpN18q/services/application/c_ifimm4gStxYSv8PR74R` - `c_ifimm4gStxYSv8PR74R` is app id)
          image: ${{ steps.meta.outputs.tags }} # e.g. ghcr.io/username/app:latest
          base_url: ${{ secrets.DOKPLOY_URL }} # e.g. https://my-dokploy-instance.com
```

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.
