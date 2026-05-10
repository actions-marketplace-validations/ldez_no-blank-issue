# no-blank-issue

![Linter](https://github.com/ldez/no-blank-issue/actions/workflows/linter.yml/badge.svg)
![CI](https://github.com/ldez/no-blank-issue/actions/workflows/ci.yml/badge.svg)
![Check dist/](https://github.com/ldez/no-blank-issue/actions/workflows/check-dist.yml/badge.svg)
![CodeQL](https://github.com/ldez/no-blank-issue/actions/workflows/codeql-analysis.yml/badge.svg)
![Coverage](./badges/coverage.svg)

A GitHub Action that automatically closes newly opened issues that have no
labels, indicating that the issue forms have not been used.

## How It Works

When a new issue is opened, this action checks whether it has at least one
label.

GitHub can automatically assign labels when using
[issue forms](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/configuring-issue-templates-for-your-repository#creating-issue-forms).

If the issue has **no labels**, the action will:

1. Post a comment explaining that the issue forms must be used.
2. Close the issue as **not planned**.
3. Lock the issue.

The message is:

```markdown
Hi [name of the issue author], thanks for opening an issue.

It looks like you did not use one of the provided issue forms when creating this
issue. To help us triage and respond efficiently, please open a new issue using
one of the
[available issue forms](https://github.com/<user/org><repo_name>/issues/new/choose).

This issue is being closed automatically.
```

## Usage

Add the following workflow to your repository at
`.github/workflows/no-blank-issue.yml`:

```yaml
name: No Blank Issues

on:
  issues:
    types:
      - opened
      - reopened

permissions:
  issues: write

jobs:
  no-blank-issue:
    name: No Blank Issue
    runs-on: ubuntu-slim

    steps:
      - name: Check New Issue
        uses: ldez/no-blank-issue@v1
        with:
          github-token: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

| Input          | Description                      | Required | Default               |
| -------------- | -------------------------------- | -------- | --------------------- |
| `github-token` | GitHub token used for API calls. | `true`   | `${{ github.token }}` |
