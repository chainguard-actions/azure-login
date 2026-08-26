<!-- markdownlint-disable -->

# Hardening Report: Azure--login/v3.0.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **Azure--login/v3.0.2** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable version tags instead of pinned 40-character commit SHAs, making them vulnerable to supply-chain attacks.

- azure-login-canary.yml: `uses: azure/login@v3` (×4)
- azure-login-integration-tests.yml: `uses: azure/login@v3` (×8), `uses: azure/powershell@v3` (×4)
- azure-login-live-tests.yml: `uses: azure/login@v3` (×1), `uses: azure/powershell@v3` (×12)
- release.yml: `uses: actions/checkout@v6`, `uses: actions/setup-node@v6`
- rollback.yml: `uses: actions/checkout@v6`

Locations:

- `.github/workflows/azure-login-canary.yml:33`
- `.github/workflows/azure-login-integration-tests.yml:15`
- `.github/workflows/azure-login-live-tests.yml:38`
- `.github/workflows/release.yml:70`
- `.github/workflows/rollback.yml:57`

### missing-permissions (severity: medium)

Three workflow files have no top-level `permissions:` block and no job-level `permissions:` blocks on any of their jobs. Without explicit permissions, workflows inherit the default (potentially broad) repository token permissions.

- ci.yml: triggered on pull_request and push, no permissions declared
- defaultLabels.yml: scheduled workflow, no permissions declared
- markdownlint.yml: triggered on push and pull_request, no permissions declared

Locations:

- `.github/workflows/ci.yml:1`
- `.github/workflows/defaultLabels.yml:1`
- `.github/workflows/markdownlint.yml:1`

### script-injection (severity: high)

Sub-rule (a): `azure-login-live-tests.yml` directly interpolates `${{ env.RG_POSITIVE }}` inside `run:` shell commands. The `env.RG_POSITIVE` value is itself constructed from `${{ github.run_id }}` (a GitHub context value), making this a direct expression interpolation in shell. Multiple `run:` steps use patterns like:
  `run: az group create --name "${{ env.RG_POSITIVE }}" --location "${{ env.LOCATION }}" --output none`
  `run: az group show --name "${{ env.RG_POSITIVE }}" --output none`
  `run: az group delete --name "${{ env.RG_POSITIVE }}" --yes --output none || true`
Any `${{ ... }}` expression directly inside a `run:` block is a script-injection risk regardless of the context it reads from.

Locations:

- `.github/workflows/azure-login-live-tests.yml:38`
- `.github/workflows/azure-login-live-tests.yml:63`
- `.github/workflows/azure-login-live-tests.yml:97`
- `.github/workflows/azure-login-live-tests.yml:116`
- `.github/workflows/azure-login-live-tests.yml:140`
- `.github/workflows/azure-login-live-tests.yml:155`
- `.github/workflows/azure-login-live-tests.yml:399`

### unsafe-shell (severity: high)

In `azure-login-live-tests.yml`, the `Positive_InDockerTest` job's 'Install Azure CLI' step pipes remote content directly to bash: `curl -sL https://aka.ms/InstallAzureCLIDeb | bash`. This executes a remotely-fetched script without first downloading and verifying it, allowing a compromised or MITM'd remote endpoint to execute arbitrary code on the runner.

Locations:

- `.github/workflows/azure-login-live-tests.yml:222`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, missing-permissions, script-injection, unsafe-shell

**Notes:**

Fixed all four findings:

1. unpinned-uses: Pinned all mutable action references to full commit SHAs:
   - azure/login@v3 → @7ddb5af1ef8758cf1353cf3b42f940aee27ba21c in canary, integration-tests, and live-tests (setup + teardown jobs)
   - azure/powershell@v3 → @f5b8adcfff1904872c7b98d4012d4914d74b1a82 in integration-tests and live-tests (all 12 occurrences across Positive_BasicTest, Positive_ParameterTest, Positive_InDockerTest, Negative_PermissionTest, Negative_ParameterTest)
   - actions/checkout@v6 → @d23441a48e516b6c34aea4fa41551a30e30af803 in release.yml and rollback.yml
   - actions/setup-node@v6 → @249970729cb0ef3589644e2896645e5dc5ba9c38 in release.yml

2. missing-permissions: Added top-level permissions blocks to ci.yml (contents: read), defaultLabels.yml (issues: write, pull-requests: write for stale action), and markdownlint.yml (contents: read).

3. script-injection: Moved all ${{ env.RG_POSITIVE }} and ${{ env.LOCATION }} expressions from run: shell strings into step-level env: blocks (as RG_NAME and RG_LOCATION), referencing them as plain shell variables in the run: scripts.

4. unsafe-shell: Replaced `curl -sL https://aka.ms/InstallAzureCLIDeb | bash` with a safe pattern: download to a mktemp file, execute the file, then remove it.

