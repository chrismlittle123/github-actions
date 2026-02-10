# github-actions

Reusable composite GitHub Actions for CI/CD across repositories.

## Actions

| Action | Description |
|--------|-------------|
| `python-standards-check` | Run `@standards-kit/conform` audit + check for Python projects |
| `python-test` | Run pytest with coverage enforcement |
| `typescript-standards-check` | Run `@standards-kit/conform` audit + check for TypeScript monorepos |
| `typescript-test` | Run vitest with coverage enforcement |
| `pulumi-deploy-aws` | Deploy infrastructure to AWS using Pulumi (OIDC) |
| `pulumi-deploy-gcp` | Deploy infrastructure to GCP using Pulumi (Workload Identity) |
| `pulumi-destroy-aws` | Destroy AWS infrastructure using Pulumi (OIDC) |

**All actions assume `actions/checkout@v4` was already called by the caller.**

## Usage

### Python Standards Check

```yaml
jobs:
  standards:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: chrismlittle123/github-actions/python-standards-check@main
        with:
          python-version: "3.13"
```

### Python Test

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: chrismlittle123/github-actions/python-test@main
        with:
          coverage-threshold: "80"
          pytest-args: "-x -q"
```

### TypeScript Standards Check

```yaml
jobs:
  standards:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: chrismlittle123/github-actions/typescript-standards-check@main
        with:
          build-command: "pnpm build"
```

### TypeScript Test

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: chrismlittle123/github-actions/typescript-test@main
        with:
          coverage-threshold: "80"
          build-command: "pnpm build"
```

### Pulumi Deploy AWS

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      - uses: chrismlittle123/github-actions/pulumi-deploy-aws@main
        with:
          role-to-assume: arn:aws:iam::123456789012:role/my-deploy-role
          pulumi-state-bucket: s3://my-pulumi-state
          environment: production
```

### Pulumi Deploy GCP

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      - uses: chrismlittle123/github-actions/pulumi-deploy-gcp@main
        with:
          workload-identity-provider: projects/123456/locations/global/workloadIdentityPools/my-pool/providers/my-provider
          service-account: deploy@my-project.iam.gserviceaccount.com
          gcp-project: my-project
          pulumi-state-bucket: gs://my-pulumi-state
          environment: production
```

### Pulumi Destroy AWS

```yaml
jobs:
  destroy:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read
    steps:
      - uses: actions/checkout@v4
      - uses: chrismlittle123/github-actions/pulumi-destroy-aws@main
        with:
          role-to-assume: arn:aws:iam::123456789012:role/my-deploy-role
          pulumi-state-bucket: s3://my-pulumi-state
          environment: staging
          timeout-minutes: "30"
```
