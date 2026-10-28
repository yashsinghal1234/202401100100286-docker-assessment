# Docker & Container Security Assessment

## Student Details
- **Name:** Yash Singhal
- **Registration Number:** `202401100100286`
- **Port:** `8086`
- **Docker Hub:** [yashsinghal307/fest-backend](https://hub.docker.com/r/yashsinghal307/fest-backend)

## Project Overview
This project demonstrates Docker containerization, security hardening, Docker Compose, vulnerability scanning using Trivy, and image signing using Cosign.

Application URL: `http://localhost:8086`

## Docker Hub Images
Repository: https://hub.docker.com/r/yashsinghal307/fest-backend

- `yashsinghal307/fest-backend:1.0.0`
- `yashsinghal307/fest-backend:1.0`
- `yashsinghal307/fest-backend:1.0.1` — unsigned verification-test image

**Version 1.0.0 digest:**

```text
sha256:148ec8566ec5607b91245bd9943ab2ef2c3504b427fb58056716cc6038d48d89
```

## Security Measures
- Backend runs as a non-root user.
- Dependencies are specified in `requirements.txt`.
- Docker Compose uses a dedicated network and persistent database volume.
- Trivy scans the container image for vulnerabilities.
- Cosign signs and verifies the published image by digest.
- `.env` and private signing keys are excluded from Git.

## Trivy Vulnerability Results

| Severity | Before | After |
|---|---:|---:|
| Critical | 0 | 0 |
| High | 6 | 5 |
| Medium | 14 | 2 |
| Low | 4 | 1 |
| **Total** | **24** | **8** |

Reported findings decreased by **66.7%**, from 24 to 8. Five High-severity findings remain and require review.

Detailed reports: `reports/trivy-before.txt` and `reports/trivy-after.txt`.

## Cosign Signing and Verification

### Sign Version 1.0.0

```bash
cosign sign --key cosign.key \
  yashsinghal307/fest-backend@sha256:148ec8566ec5607b91245bd9943ab2ef2c3504b427fb58056716cc6038d48d89
```

### Verify the Signed Image

```bash
cosign verify --key cosign.pub \
  yashsinghal307/fest-backend@sha256:148ec8566ec5607b91245bd9943ab2ef2c3504b427fb58056716cc6038d48d89
```

Verification succeeded for version `1.0.0`.

### Verify the Unsigned Version 1.0.1

```bash
cosign verify --key cosign.pub \
  yashsinghal307/fest-backend@sha256:7dd29a686e10279d2d1dcb6bbf2ebeec0828f5e75ae33692859a813932991ec1
```

Expected result: `no signatures found`. This confirms the unsigned test image could not be verified with the public key.

## Submission Checklist
- Include `cosign.pub`, `.gitignore`, Docker configuration, source files, scan reports, and required assessment screenshots.
- Ensure `.env` and `cosign.key` are not tracked by Git or present in Git history.
- Include evidence for Stages 1–5 as required by the assessment instructions.
- Verify the GitHub repository and Docker Hub links before submission.

## AI Tools Disclosure
ChatGPT was used for technical troubleshooting, Docker and Cosign guidance, interpreting Trivy results, and documentation assistance.

## Conclusion
The project demonstrates container hardening, vulnerability scanning, and image signature verification. Trivy findings decreased from 24 to 8, and Cosign successfully verified the signed image while rejecting the unsigned test image.
