# DOTcom404.
name: Node.js CI

on:
  push:
    branches: [ "main" ]
    paths:
      - 'next/**'
  pull_request:
    branches: [ "main" ]
    paths:
      - 'next/**'

jobs:
  build:
    runs-on: ubuntu-latest
    defaults:
      run:
        working-directory: next
    steps:
      - uses: actions/checkout@v3
      - name: Use Node.js 18
        uses: actions/setup-node@v3
        with:
          node-version: 18.x
          cache: "npm"
          cache-dependency-path: next/package-lock.json
      - run: npm ci
      - run: npm test
        env:
          OPENAI_API_KEY: sk-0000000000
      - run: ./prisma/useSqlite.sh && npm run postinstall404
