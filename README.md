# k6-load-testing-examples

A basic k6 load test script and a GitHub Actions workflow to run it on demand against any URL.

## Run locally

```bash
k6 run scripts/basic-load-test.js -e BASE_URL=http://localhost:3000
```

Ramps to 20 virtual users over 30s, holds for 1 minute, ramps down — and fails the run if p95 latency exceeds 500ms or more than 1% of requests error out.

## Run from GitHub Actions

Go to Actions → "k6 load test" → Run workflow, and pass the target URL. Useful for a quick load-test pass against a staging environment without provisioning anything.

### Setup

Create `.github/workflows/k6.yml` in this repo (via the GitHub web UI: **Add file → Create new file**) with:

```yaml
name: k6 load test

on:
  workflow_dispatch:
    inputs:
      base_url:
        description: "Target URL to load test"
        required: true

jobs:
  load-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run k6
        uses: grafana/k6-action@v0.3.1
        with:
          filename: scripts/basic-load-test.js
        env:
          BASE_URL: ${{ inputs.base_url }}
```
