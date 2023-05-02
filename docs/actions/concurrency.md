# GitHub Actions - Concurrency

## General

GitHub Actions allows you to optimize the execution time of your workflows by using the `concurrency` keyword. This keyword can be used at the workflow scope, or within a specific job, to specify how many actions can run simultaneously. In this blog post, we will explore the use of the `concurrency` keyword in GitHub Actions and provide some examples to make it clearer.

## Example

The example we'll look at can be found [**here**](https://github.com/christosgalano/GitHub-Actions-Deep-Dive/blob/main/.github/workflows/concurreny.yaml):

```yaml
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
      - uses: actions/checkout@v3
      - run: |
          echo "Hello world 1!"
          sleep 5
          echo "Hello world 1 again!"

  hello-world-2:
    runs-on: ubuntu-latest
    concurrency: hello-world # same group with hello-world-1
    steps:
      - uses: actions/checkout@v3
      - run: |
          echo "Hello world 2!"
          sleep 5
          echo "Hello world 2 again!"

  hello-world-3:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - run: |
          echo "Hello world 3!"
          sleep 5
          echo "Hello world 3 again, no waiting here!"

```

Here, the workflow is triggered manually. It has 3 jobs, which simply print a message, sleep for five seconds, and then print another message.

Let's first analyze the `concurrency` keyword on a job scope. We can see that both the `hello-world-1` and the `hello-world-2` belong to the same concurrency group, which is called `hello-world`. So, when we trigger the workflow, we expect two jobs to run simultaneously, one of which will be `hello-world-3` since it does not belong to any concurrency group and does need to wait for another job to finish. A possible sequence of execution is the following one:

![concurrency-job-scope](../../images/concurrency-job-scope.png)

Now it's time to understand the `concurrency` keyword on a workflow scope. The preceding workflow is a member of the concurrency group `hello-world-${{ github.ref }}`. The option `cancel-in-progress` gives someone the ability to cancel all on-going jobs and workflows that belong in the specified group and only run the currently triggered one.

This is an example output with `cancel-in-progress: false`:

![concurrency-workflow-scope-no-cancel](../../images/concurrency-workflow-scope-no-cancel.png)

And this is an example output with `cancel-in-progress: true`:

![concurrency-workflow-scope-with-cancel](../../images/concurrency-workflow-scope-with-cancel.png)

## Summary

In this blog post, we discussed how to use the `concurrency` keyword in GitHub Actions to speed up workflow execution. By allowing multiple actions to run simultaneously, concurrency can greatly reduce the overall execution time of a workflow. We explained how to set concurrency at the top level of the workflow file, within a specific job, or within a group of jobs, and also provided an example showcasing all of the above.

## Resources

- **Related repository:** [GitHub-Actions-Deep-Dive](https://github.com/christosgalano/GitHub-Actions-Deep-Dive)
- **Related documentation:** [Using concurrency](https://docs.github.com/en/actions/using-jobs/using-concurrency)
