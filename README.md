## Onboarding release-please

1. Add `.github/workflows/release-please.yaml` (ensure that default branch name is correct)
2. Add `release-please-config.json` (ensure that package name for uv.lock update is correct)
3. Add `.release-please-manifest.json` with the current version of the project (e.g., `0.1.0`).
4. Adapt repo settings:
    - Disable "Allow merge commits" and "Allow rebase merging"
    - Enable "Automatically delete head branches"

    You may do this with curl:
    ```bash
    export GITHUB_TOKEN="ghp_..."
    export GITHUB_REPO="owner/repo"
    curl -X PATCH \
        -H "Authorization: Bearer $GITHUB_TOKEN" \
        -H "Accept: application/vnd.github+json" \
        "https://api.github.com/repos/$GITHUB_REPO" \
        -d '{
      "allow_merge_commit": false,
      "allow_rebase_merge": false,
      "delete_branch_on_merge": true
    }'
    ```

## 🚀 Basic Usage

<!-- x-release-please-start-version -->
```yaml
- name: Check vulnerabilities of latest release/tag
  uses: mxmehl/latest-release-vulnerability-status@v2.1.0
```
<!-- x-release-please-end -->

## 🔧 Inputs

| Name         | Description                                          | Required | Default                     | Example                    |
| ------------ | ---------------------------------------------------- | -------- | --------------------------- | -------------------------- |
| `repository` | The repo to scan (`owner/repo` format)               | ❌       | `user/repo` of current repo | `mxmehl/leaky-poetry-repo` |
| `tag-source` | Source of latest version: `release-api` or `git-tag` | ❌       | `release-api`               | `git-tag`                  |

## 📤 Output

The result is reflected in:

- the GitHub Actions Summary
- and the step's exit status (non-zero if vulnerabilities found or scan failed).

As an example, you may want to check [the workflow runs of this very repository](https://github.com/mxmehl/latest-release-vulnerability-status/actions) in which the action tests itself. Note: errors are explictly ignored as per step definition which is not the default.

## 🧪 Example workflow

<!-- x-release-please-start-version -->
```yaml
jobs:
  osv-check:
    runs-on: ubuntu-latest
    # Recommended: Only run this job in upstream repository, not forks
    if: github.repository_owner == 'YOUR_GITHUB_USERNAME' # <-- change value!
    steps:
      - name: Check vulnerabilities of latest release/tag
        uses: mxmehl/latest-release-vulnerability-status@v2.1.0
        # with:
        #   repository: mxmehl/leaky-poetry-repo  # Optional: username/reponame
        #   tag-source: git-tag  # Optional, defaults to release-api
        # continue-on-error: true  # Optional: ignore scan failures (e.g. if vulnerabilities are found)
```
<!-- x-release-please-end -->
