# Pull Request Commands - Simple

When the command is started, a 🚀 is added as a reaction on the comment, and when the command is finished and changes are committed, a 🎉 is added as a reaction. If the command fails, 😕 is added as a reaction.

This can be used for anything, but examples include:
- `!pr lint` lints the code.
- `!pr update-screenshots` updates screenshots for visual regression tests.
- `!pr coverage` generates coverage reports
- `!pr dedup` deduplicates yarn packages in `yarn.lock`.

# Usage
<!-- start usage -->
```yaml
name: <insert action name>

on:
  issue_comment:
    types: [created]

jobs:
  <insert action name>:
    runs-on: ubuntu-latest
    steps:
      - uses: siteimprove/pr-command/simple@v2
        with:
          # Personal access token (PAT) used to fetch the repository and add reaction on comment (See note about token)
          token: ''
          # Comment that the action should be triggered on.
          # Note: if the comment contains special characters it needs to be wrapped as a string ''
          # Example: '!pr lint'
          pr-comment: ''
          # Command to be run when the comment has been written
          run: ''
          # Commit message of the changes
          commit-message: ''
          # This gets written as a comment on the PR
          write-comment: ''
```
<!-- end usage -->

## Options

| Option                    | Description                                                | Required       |
| ------------------------- | ---------------------------------------------------------- | -------------- |
| **`token`**               | Personal access token (PAT) used to fetch the repository.  | `true`         |
| **`pr-comment`**          | Comment that the action should be triggered on.            | `true`         |
| **`run`**                 | Command to be run when the comment has been written        | `false`        |
| **`commit-message`**      | Commit message of the changes                              | `false`        |
| **`write-comment`**       | A comment that the bot should write on the PR              | `false`        |

## Note: Token
An authentication token is required in order to fetch the repository and add reactions on the comment.
If you use the action in a personal repository, you can create a [PAT](https://github.com/settings/tokens) (Personal access token).
If you use the action in an organization, you probably want to use a PAT from an organizational user. 
> Notice: If you use an organizational user, it needs to be able to access the repository.


Normally the `GITHUB_TOKEN` would have been sufficient, but Github Actions blocks that in order to prevent recursive workflow runs.

> When you use the repository's GITHUB_TOKEN to perform tasks, events triggered by the GITHUB_TOKEN will not create a new workflow run. This prevents you from accidentally creating recursive workflow runs. - [Github Action docs](https://docs.github.com/en/enterprise-server@3.0/actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow)


## Example
```yaml
name: Lint code

on:
  issue_comment:
    types: [created]

jobs:
  lint-code:
    runs-on: ubuntu-latest
    steps:
      - uses: siteimprove/pr-command/simple@v2
        with:
          token: ${{ secrets.PAT }}
          pr-comment: '!pr lint'
          run: |
            npm ci
            npm run lint:fix
          commit-message: Lint code
```
