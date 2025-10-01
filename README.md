# dependabot-bundler-conservative

A custom action to update Dependabot's pull request for bundler with `--conservative` option.

When Dependabot creates a pull request to update `Gemfile.lock`, it typically updates both direct and indirect dependencies. This can lead to larger changes that are harder to review.

By adjusting the suggested `Gemfile.lock` diff so that changes to indirect dependencies are minimized, the resulting changes become smaller and each pull request is easier to review and accept.

## Usage

Here is an example of how to use this action in a GitHub Actions workflow:

```yaml
# .github/workflows/dependabot-bundler-conservative.yml
name: dependabot-bundler-conservative

on:
  pull_request:
    paths:
      - Gemfile.lock

jobs:
  dependabot-bundler-conservative:
    permissions:
      pull-requests: read
    runs-on: ubuntu-latest
    if: github.actor == 'dependabot[bot]'
    steps:
      - uses: r7kamura/dependabot-bundler-conservative@v0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN_WITH_REQUIRED_PERMISSIONS }}
```

Also, please configure the secrets so that GitHub Actions workflows triggered by Dependabot are provided with an access token that has the appropriate permissions.

## Inputs

### `github-token`

The access token used by this custom action requires the following permissions:

- `contents:write`
- `pull-requests:read`
- `workflow:write`
