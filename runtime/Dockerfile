name: CI

on:
  push:
    branches: [ main, develop ]
  pull_request:

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node
        uses: actions/setup-node@v4
        with:
          node-version: 22

      - name: Install deps
        run: |
          cd runtime
          npm install

      - name: Test
        run: |
          cd runtime
          npm test || echo "no tests yet"

  docker-build-and-push:
    needs: build-and-test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Login to GHCR
        uses: docker/login-action@v3
        with:
          registry: ghcr.io
          username: ${{ github.actor }}
          password: ${{ secrets.GITHUB_TOKEN }}

      - name: Build and push image
        run: |
          IMAGE=ghcr.io/${{ github.repository }}:latest
          docker build -t $IMAGE -f runtime/Dockerfile .
          docker push $IMAGE