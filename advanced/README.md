# Pull Request Commands - Advanced

All commands are configurable through a config file which needs to be located in `.pr-commands/config.json`.

If the command fails, 😕 is added as a reaction.

A special command `help` is included e.g. `!pr help`, which will write all possible commands in a comment.

This can be used for anything, but examples include:
- `!pr lint` lints the code.
- `!pr update-screenshots` updates screenshots for visual regression tests.
- `!pr coverage` generates coverage reports
- `!pr dedup` deduplicates yarn packages in `yarn.lock`.

# Usage
**.pr-commands/config.json**
```json
{
    "test": { //Test is the command key
        "command": "echo test", // Executed through bash (optional)
        "description": "Hello!", // Visible in the help text
        "startEmoji": "+1", // Defaults to rocket(🚀), disable with null
        "successEmoji": "heart", // Defaults to hooray(🎉), disable with null
        "commitMsg": "test 123",
        "commentMsg": "This is my output: <COMMAND_RESULT>" // The <COMMAND_RESULT> token can be used to replace with output of command run.
    }
}

```
<!-- start usage -->
**.github/workflows/pr-command.yaml**
```yaml
name: <insert action name>

on:
  issue_comment:
    types: [created]

jobs:
  <insert action name>:
    runs-on: ubuntu-latest
    steps:
      - uses: siteimprove/pr-command/advanced@v2
        with:
          # Personal access token (PAT) used to fetch the repository and add reaction on comment (See note about token)
          token: ''
```
<!-- end usage -->

## Options

| Option                    | Description                                                | Required       |
| ------------------------- | ---------------------------------------------------------- | -------------- |
| **`token`**               | Personal access token (PAT) used to fetch the repository.  | `true`         |
| **`prefix`**              | The command prefix which is used in messages, default: !pr | `false`        |


## Note: Token
An authentication token is required in order to fetch the repository and add reactions on the comment.
If you use the action in a personal repository, you can create a [PAT](https://github.com/settings/tokens) (Personal access token).
If you use the action in an organization, you probably want to use a PAT from an organizational user. 
> Notice: If you use an organizational user, it needs to be able to access the repository.


Normally the `GITHUB_TOKEN` would have been sufficient, but Github Actions blocks that in order to prevent recursive workflow runs.

> When you use the repository's GITHUB_TOKEN to perform tasks, events triggered by the GITHUB_TOKEN will not create a new workflow run. This prevents you from accidentally creating recursive workflow runs. - [Github Action docs](https://docs.github.com/en/enterprise-server@3.0/actions/security-guides/automatic-token-authentication#using-the-github_token-in-a-workflow)


## Example
**.pr-commands/config.json**
```json
{
    "lint": {
        "command": "npm ci && npm run lint:fix",
        "description": "Lints the code",
        "commitMsg": "Lint code"
    }
}
```
**.github/workflows/pr-command.yaml**
```yaml
name: PR commands

on:
  issue_comment:
    types: [created]

jobs:
  lint-code:
    runs-on: ubuntu-latest
    steps:
      - uses: siteimprove/pr-command/advanced@v2
        with:
          token: ${{ secrets.PAT }}
```
