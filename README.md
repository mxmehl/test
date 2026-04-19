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
