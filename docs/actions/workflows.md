# GitHub Actions - Workflows

## General

As we learned, GitHub Actions is a powerful tool for automating software development workflows. One of the key features of GitHub Actions is the ability to create workflows that are triggered by specific events, such as a push to a specific branch or the opening of a pull request. Here we will explore the concepts of triggers, jobs, and steps in GitHub Actions, and provide examples of how to use these features to create powerful and efficient workflows.

## Triggers

Triggers are the events that initiate a workflow in GitHub Actions. When a trigger event occurs, GitHub Actions will automatically start the workflow associated with that event. There are several types of trigger events that can be used to start a workflow, including:

- `push`: Triggers the workflow when a new commit is pushed to a branch.

- `pull_request`: Triggers the workflow when a new pull request is opened, or when an existing pull request is updated.

- `schedule`: Triggers the workflow on a specified schedule, such as daily or weekly.

- `workflow_dispatch`: Allows manual triggering of a workflow via the GitHub Actions API.

For example, here is a simple workflow that is triggered by a push to the `main` branch:

```yaml
name: build-deploy
on:
  push:
    branches:
    - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v3
    - name: Build
      run: make build
    - name: Deploy
      run: make deploy
```

In this example, the workflow is triggered by a push to the `main` branch. When a new commit is pushed to the `main` branch, GitHub Actions will automatically start the build job, which consists of three steps: checking out the repository, building, and then deploying the code.

## Jobs

Jobs are the units of work in a GitHub Actions workflow. A job is a series of steps that are run in order, and each job runs on a specific runner. Jobs can run in parallel with other jobs and can be defined using the `jobs` keyword.

Each job is defined within a jobs block, and can contain one or more steps. Jobs can also have multiple runs on different operating systems or different versions, that's where the `strategy` and  `matrix` keywords come in handy.

For example, here is a simple workflow that runs two jobs in parallel:

```yaml
name: build-test-deploy
on: push

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Build
      run: make build
  test:
    runs-on: ubuntu-latest
    steps:
    - name: Test
      run: make test
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Deploy
      run: make deploy
```

Here, all the jobs will run in parallel, rather than waiting for one to finish before starting the other. This can greatly speed up your workflow, as the build, test, and deploy steps can run simultaneously, rather than sequentially.

If we wanted to have linear execution based on dependencies, then we would need to use the `needs` keyword. Let's say we want to modify the `build-test-deploy` so that the `test` job executes only if the `build` completes successfully, and also the `deploy` job executes only if the `test` completes without errors:

```yaml
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Build
      run: make build
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
    - name: Test
      run: make test
  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
    - name: Deploy
      run: make deploy
```

## Steps

Steps are defined using the `steps` keyword, and each step can contain one or more actions. There are several types of actions that can be used in steps, including:

- `run`: Runs a command in the terminal.

- `uses`: Runs a pre-built action or a Docker container.

- `env`: Sets environment variables for the step.

- `if`: Allows conditional execution of steps based on the success or failure of previous steps.

Here's an example of a step that runs a command, sets an environment variable, and runs two conditional steps:

```yaml
steps:
  - name: Checkout
    uses: actions/checkout@v3

  - name: Set environment variable
    env:
      FOO: bar

  - name: Run tests
    run: make test

  - name: Generate report
    if: failure()
    run: report
```

Here, the first step checks out the code from the repository, the second step sets an environment variable, the third step runs tests, and the fourth step generates a report, but only if the previous step of running tests failed.

It's also worth noting that there are many pre-built actions available in the [GitHub Actions marketplace](https://github.com/marketplace) that can be used to perform common tasks, such as deploying to a specific platform or sending notifications. These actions can be easily integrated into your workflow using the uses action.

## Summary

In summary, GitHub Actions workflows consist of triggers, jobs, and steps. Triggers initiate the workflow and determine when it should run. Jobs are the units of work in a workflow and are made up of one or more steps. Steps are the individual tasks that make up a job and can include a variety of actions, such as running commands, checking out code, and deploying packages. By understanding how triggers, jobs, and steps work together, you can create powerful and efficient workflows that automate your software development process and save valuable time.

## Resources

- [**GitHub Actions - Workflows**](https://christosgalano.github.io/github/github-actions-workflows/)
- [**About workflows**](https://docs.github.com/en/actions/using-workflows/about-workflows)
