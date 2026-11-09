# ds-identity-witness

Minimal GitHub Actions witness repo for the DS-Hns execution-identity capture
(approved author path O1: "use the logged-in GitHub account permissions via
Actions").

The `witness` workflow, on push to `main` or manual dispatch:

1. hashes the workflow file that actually executes (`.github/workflows/witness.yml`);
2. requests a GitHub Actions OIDC token for audience `ds-hns-identity`;
3. writes `attestation.json` with run/attempt/head SHA, workflow path + SHA-256,
   job, event and the OIDC token itself;
4. uploads the attestation as a run artifact.

The capture side verifies the token signature against GitHub's JWKS
(`https://token.actions.githubusercontent.com/.well-known/jwks`), checks the
claims against the run metadata, and only then marks the capture identity as
`verified`. This repository exists solely to make that binding observable; it
holds no research data.
