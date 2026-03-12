# actions-trivy-scan

Composite GitHub Action that pulls a Docker image from AWS ECR and scans it for vulnerabilities using Trivy.

Requires Trivy and AWS CLI to be pre-installed on the runner.

## Usage

```yaml
- name: Trivy Vulnerability Scan
  uses: devops-looplava/actions-trivy-scan@main
  with:
    ECR_USERNAME: ${{ secrets.ECR_USERNAME }}
    REPO: ${{ github.repository }}
    IMAGE_TAG: pull-amd-${{ github.sha }}
```

Scan without failing the build:

```yaml
- name: Trivy Vulnerability Scan (advisory)
  uses: devops-looplava/actions-trivy-scan@main
  with:
    ECR_USERNAME: ${{ secrets.ECR_USERNAME }}
    REPO: ${{ github.repository }}
    IMAGE_TAG: pull-amd-${{ github.sha }}
    EXIT_CODE: "0"
    SEVERITY: "CRITICAL"
```

## Inputs

| Input | Required | Default | Description |
|-------|----------|---------|-------------|
| `ECR_USERNAME` | Yes | - | AWS ECR registry URL |
| `ECR_REGION` | No | `us-west-2` | AWS region for ECR |
| `REPO` | Yes | - | Full GitHub repository (`owner/name`) |
| `IMAGE_TAG` | Yes | - | Image tag to scan |
| `SEVERITY` | No | `CRITICAL,HIGH` | Severity levels to check |
| `EXIT_CODE` | No | `1` | Exit code when vulnerabilities found (`0` = don't fail) |
| `SKIP_FILES` | No | `/usr/local/lib/node_modules/**` | File paths to skip |
| `IGNORE_UNFIXED` | No | `true` | Ignore unfixed vulnerabilities |

## What it does

1. Logs into AWS ECR
2. Pulls the specified Docker image
3. Runs `trivy image` with configured severity and options
