# Central GitHub Actions templates

Reusable GitHub Actions workflows shared by Terraform service/module repositories.

## Available workflows

### Release a Terraform module to Nexus

Caller repositories invoke:

```yaml
jobs:
  release:
    uses: manojrasia/terraform-github-actions-templates/.github/workflows/terraform-module-release.yml@terraformgcp
    with:
      module_name: terraform-google-gcs
      terraform_version: "1.9.8"
      nexus_repository: ${{ vars.NEXUS_RAW_REPOSITORY }}
    secrets:
      NEXUS_URL: ${{ secrets.NEXUS_URL }}
      NEXUS_USERNAME: ${{ secrets.NEXUS_USERNAME }}
      NEXUS_PASSWORD: ${{ secrets.NEXUS_PASSWORD }}
```

The reusable workflow checks out the caller repository, runs Terraform fmt/init/validate, packages the module root files, creates a SHA-256 checksum, and uploads both files to Nexus.

## Required runner and configuration

- The caller repository must have access to this repository under **Settings → Actions → General → Access**.
- Configure the caller's GitHub Actions secrets: `NEXUS_URL`, `NEXUS_USERNAME`, and `NEXUS_PASSWORD`.
- Configure the caller's Actions variable: `NEXUS_RAW_REPOSITORY`.
- The self-hosted runner must have the labels `self-hosted`, `linux`, and `nexus-local`, plus `zip`, `curl`, and network access to Nexus.
- Pin callers to a protected release tag or immutable commit SHA for production use. The branch ref above is convenient for development.
