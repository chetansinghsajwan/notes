# Workflow

A workflow is a sequence of tasks that is run on an event.

Each workflow is stored as a separate YAML file in your code repository, in a directory named `.github/workflows`.

A workflow must contain the following basic components:

- One or more _events_ that will trigger the workflow.

- One or more _jobs_, each of which will execute on a _runner_ machine and run a series of one or more _steps_.

- Each step can either run a script that you define or run an action, which is a reusable extension that can simplify your workflow.

## Triggering a workflow

Workflow triggers are events that cause a workflow to run. These events can be:

- Events that occur in your workflow's repository
- Events that occur outside of GitHub and trigger a `repository_dispatch` event on GitHub
- Scheduled times
- Manual

For example, you can configure your workflow to run when a push is made to the default branch of your repository, when a release is created, or when an issue is opened.

## Workflow Syntax

Workflow are defined using YAML. For the full reference of the YAML syntax for authoring workflows, see [Workflow syntax for GitHub Actions](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#about-yaml-syntax-for-workflows).

## References

- https://docs.github.com/en/actions/using-workflows/about-workflows
