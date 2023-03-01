# Pull Request Commands - Separate check and run steps

This version splits the actions in two parts: one that checks whether the action should run and does some basic setup (checkout the repository, handle the `help` command, …); the other that performs the actual action. Between the two actions, it is possible to interpose repository-specific commands, especially the ones that are used by all PR Commands flow (e.g., `actions/setup-node`, `install`, `build`, or `test`, …)

The `config.json` file, options, and token restriction follows the exact same syntax as for the [advanced action](../advanced).

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
      - name: "Should PR Commands run?"
        uses: siteimprove/pr-command/check-run/check@v2
        with:
          # Personal access token (PAT) used to fetch the repository and add reaction on comment (See note about token)
          token: ''

      # At this point, repository specific setup can be performed before running the actual PR commands.
      # The repository has been checked out, on the latest commit on the PR that triggered the workflow.
      # All steps should be conditional with "if: env.PR_COMMAND_WILL_RUN == 'true'" to ensure they are
      # only run if needed.
      # The 'check' action has set two environment variables:
      # * PR_COMMAND_WILL_RUN is 'true' iff check determined the comment triggers an existing command.
      # * PR_COMMAND_DETAILS is a JSON object containing the details of the command, as read from the config file.
      
      - name: "Conditional stuff"
        if: env.PR_COMMAND_WILL_RUN == 'true'
        run: |
          echo "I should probably do stuff"
          echo $PR_COMMAND_DETAILS
      
      - name: "Do stuff"
        uses: siteimprove/pr-command/check-run/run@v2
        with:
          # Personal access token (PAT) used to fetch the repository and add reaction on comment (See note about token)
          token: ''
```
<!-- end usage -->
