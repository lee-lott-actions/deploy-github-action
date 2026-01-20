# Create Major Version Tag Action

This GitHub Action creates (or updates) a major version tag (such as `v1`, `v2`, etc.) that points to a specific release tag in your repository. It's commonly used for GitHub Actions repositories to ensure that tag references like `@v1` always resolve to the latest major release, simplifying action upgrades for users. The action deletes any existing major version tag before creating the new one, guaranteeing a consistent, up-to-date major tag.

---

## Features

- Derives the major version tag (`v1`, `v2`, ...) from a full semantic version tag (e.g., `v1.2.3`).
- Deletes the existing major version tag if it exists.
- Creates a new annotated tag pointing to the given branch or commit SHA.
- Outputs the result and generates a job summary for transparency.
- Designed for GitHub Actions and reusable by other automation workflows.

---

## Inputs

| Name           | Description                                                               | Required |
|----------------|---------------------------------------------------------------------------|----------|
| `version-tag`  | The tag to derive the new major version tag from (e.g., `v1.2.3`).        | Yes      |
| `branch-name`  | The branch name on which to create the tag (e.g., `main`).                | Yes      |
| `commit-sha`   | Optionally specify the commit SHA to tag instead of the branch head.      | No       |
| `org-name`     | The name of the GitHub organization.                                      | Yes      |
| `repo-name`    | The name of the repository.                                               | Yes      |
| `token`        | GitHub token with access to Git tags.                                     | Yes      |

---

## Usage

**Example Workflow:**

```yaml
name: Update Major Version Tag

on:
  workflow_dispatch:

jobs:
  major-version-tag:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v5

      - name: Create/Update Major Version Tag
        uses: lee-lott-actions/create-major-version-tag@v1
        with:
          version-tag: 'v1.2.3'
          branch-name: 'main'
          org-name: 'your-org'
          repo-name: 'your-repo'
          token: ${{ secrets.GITHUB_TOKEN }}
```

---

## How it Works

1. **Derive Major Version**: Extracts the major version (e.g., `v1`) from the given `version-tag` using semantic versioning.
2. **Delete Old Tag**: Deletes the existing major version tag (if present) to avoid conflicts.
3. **Create New Tag**: Creates a new annotated tag at the latest commit on the specified branch (or at the SHA if provided).
4. **Report Summary**: Adds an entry to the GitHub job summary reporting the result of the operation.

---

## Notes

- **Typical Use Case:** After creating a new release (e.g., `v2.5.0`), run this action to update the `v2` tag automatically.
- **Best Practices:** Reference actions in workflows by the major version tag (e.g., `your-action@v1`). Keeping the major tag updated ensures users always get your latest compatible release for that major version.
- The commit SHA is optional. If not specified, the tag is created at the current head of the provided branch.
- The action requires delete and create tag permissions for the supplied `token`.

---
