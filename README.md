## Next.js CI / CD on AWS with GitHub Actions and the Serverless Framework

This is an example project showing how to integrate a CI / CD pipeline for your Next.js app on AWS.

This project uses the [Serverless Next.js Component](https://github.com/serverless-nextjs/serverless-next.js) to deploy a Next.js app to AWS complete with Cloudfront and Lambda at Edge – enabling you to take advantage of Next.js features like SSR, fallback routes, API routes, and revalidation.

## About the project

The main two files to keep in mind here are __serverless.yml__ and __.github/workflows/main.yml__.

### serverless.yml

This file configures the Next.js Serverless Component

```yml
slsNextApp:
  component: "@sls-next/serverless-component@1.18.0"
```

### .github/workflows/main.yml

This file enables the GitHub action

```yml
name: CI
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
  workflow_dispatch:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v1
      - uses: actions/setup-node@v1
        with:
          node-version: '12.x'
      - name: Install NPM dependencies
        run: npm install
      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v1
        with:
          aws-access-key-id: ${{ secrets.AWS_KEY }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET }}
          aws-region: us-east-1
      - name: Deploy Next.js app
        run: npx serverless
```