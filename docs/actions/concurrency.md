# GitHub Actions: Concurrency

> **Managing concurrency in GitHub Actions**

## Overview

As an engineer, you understand the importance of managing concurrency in your workflow to optimize your development process and improve the efficiency and speed of your code. GitHub Actions provides a powerful workflow automation system that allows you to run multiple jobs and workflows simultaneously. However, managing concurrency in GitHub Actions can be a complex task, and it's essential to understand the best practices for optimizing your workflow.

## The concurrency keyword

The `concurrency` keyword can be used to make sure that only one job or workflow using a given concurrency group is running at once. Any string or phrase can be a concurrency group. Only the github context may be used by the expression. For instance, you can build a concurrency group for each branch in your repository using the `github.ref` context. Without a concurrency group, the job or process will start up right away.

You can also specify concurrency at the job level. If you do that, it will override the concurrency group specified at the workflow level.

If another job or workflow utilizing the same concurrency group in the repository is running when a concurrent job or workflow is queued, the queued job or workflow will be in the pending state. The concurrency group will cancel any open jobs or workflows that were previously in progress. With the `cancel-in-progress: true` option, you can also halt any active jobs or workflows in the same concurrency group.

## Example

Let's take a look at an example:

```yaml
# .github/workflows/concurrency.yaml
name: concurrency
on:
  workflow_dispatch:

concurrency:
  group: hello-world-${{ github.ref }}
  cancel-in-progress: false

jobs:
  hello-world-1:
    runs-on: ubuntu-latest
    concurrency: hello-world # same group with hello-world-2
    steps:
      - uses: actions/checkout@v4
      - run: |
          echo "Hello world 1!"
          sleep 5
          echo "Hello world 1 again!"

  hello-world-2:
    runs-on: ubuntu-latest
    concurrency: hello-world # same group with hello-world-1
    steps:
      - uses: actions/checkout@v4
      - run: |
          echo "Hello world 2!"
          sleep 5
          echo "Hello world 2 again!"

  hello-world-3:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: |
          echo "Hello world 3!"
          sleep 5
          echo "Hello world 3 again, no waiting here!"

```

Here, the workflow is triggered manually. It has 3 jobs, which simply print a message, sleep for five seconds, and then print another message.

Let's first analyze the `concurrency` keyword on a job scope. We can see that both the `hello-world-1` and the `hello-world-2` belong to the same concurrency group, which is called `hello-world`. So, when we trigger the workflow, we expect two jobs to run simultaneously, one of which will be `hello-world-3` since it does not belong to any concurrency group and does need to wait for another job to finish. A possible sequence of execution is the following one:

![concurrency-job-scope](/assets/images/actions/concurrency-job-scope.png)

Now it's time to understand the `concurrency` keyword on a workflow scope. The preceding workflow is a member of the concurrency group `hello-world-${{ github.ref }}`. The option `cancel-in-progress` gives someone the ability to cancel all on-going jobs and workflows that belong in the specified group and only run the currently triggered one.

This is an example output with `cancel-in-progress: false`:

![concurrency-workflow-scope-no-cancel](/assets/images/actions/concurrency-workflow-scope-no-cancel.png)

And this is an example output with `cancel-in-progress: true`:

![concurrency-workflow-scope-with-cancel](/assets/images/actions/concurrency-workflow-scope-with-cancel.png)

## Summary

In summary. the `concurrency` keyword can be used to make sure that only one job or workflow using a given concurrency group is running at once. You can  set concurrency at the top level of the workflow file, within a specific job, or within a group of jobs.

## References

- [**Using concurrency**](https://docs.github.com/en/actions/using-jobs/using-concurrency)

## Next

- [**Custom Actions**](./custom_actions.md)
