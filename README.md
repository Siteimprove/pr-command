# Pull Request Commands

`pr-command` is an action that enables you to run commands on your pull requests using comments.

This action will be triggered on comments on pull requests: it checks-out your pull request, runs a command, and commits the result back to the branch. It can also write a comment on the PR.

There is:
* a [simple action](simple/README.md), reacting to a given comment with a given command;
* an [advanced action](advanced/README.md), reacting to comments starting with a given prefix, with commands that depend on the actual comment;
* a composite [two-steps action](check-run/README.md), separating the process of the comment and the run of the command to leave room for extra setup should a command be run.

![Example](https://user-images.githubusercontent.com/25243461/150767980-d6c82e0a-e8a6-4e9e-8e29-07a2133ee65c.png)
