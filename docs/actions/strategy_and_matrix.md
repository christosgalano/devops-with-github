# GitHub Actions - Strategy & Matrix

## General

The `strategy` and `matrix` keywords in GitHub Actions are powerful tools for implementing parallel workflows. These keywords allow you to define multiple combinations of inputs that should be run in parallel, greatly speeding up your workflow and increasing its reliability. In this blog post, we will explore the strategy and matrix keywords in depth, and provide examples of how to use them to implement parallel workflows in your own projects.

The `strategy` keyword is used to define a parallel strategy for your workflow. It is typically used in conjunction with the `matrix` keyword, which allows you to define multiple combinations of inputs that should be run in parallel. The strategy keyword takes a single object, which can contain the following properties:

- `matrix`: This property is used to define a matrix of inputs that should be run in parallel. The matrix property takes an object, where the keys are the names of the inputs and the values are arrays of possible values for each input. Using the `matrix` keyword in this way can greatly improve the reliability of your workflow, as it allows you to test against a wide range of inputs and identify any potential issues before they are released to production. Another important aspect of the `matrix` is that its values are available and can be used in the steps of a job. Finally, it's also worth noting that the `matrix` can be defined in multiple jobs, and it's also possible to use it in conjunction with `if` statements to conditionally run certain jobs based on the matrix values.

- `fail-fast`: This property is used to specify whether the workflow should fail as soon as one job fails. The default value is false, which means that the workflow will continue running even if one job fails.

- `max-parallel`: This property is used to specify the maximum number of jobs that should run in parallel.

## Example

The example we'll look at can be found [**here**](https://github.com/christosgalano/GitHub-Actions-Deep-Dive/blob/main/.github/workflows/strategy_matrix.yaml):

```yaml
name: strategy-matrix
on:
  workflow_dispatch:

jobs:
  strategy-matrix:
    # specify bash as default shell on all os
    defaults:
      run:
        shell: bash

    strategy:
      max-parallel: 5
      fail-fast: false
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        version: [8, 10, 12]
        browser: [chrome, firefox, safari]

    runs-on: ${{ matrix.os }}

    steps:
      - name: Print info
        run: |
          echo "OS:" ${{ matrix.os }}
          echo "Version:" ${{ matrix.version }}
          echo "Browser:" ${{ matrix.browser }}
```

Here, the matrix specifies that the workflow should run on three different operating systems (ubuntu-latest, windows-latest, and macos-latest), three different versions (8, 10, and 12) and three different browsers (chrome, firefox, safari). This means that the workflow will run a total of 27 (3^3) combinations in parallel, rather than sequentially.

We also put a limit of 5 to how many jobs can run simultaneously by using `max-parallel: 5`.

Finally, we do not want all of the remaining jobs to be cancelled if a matrix-job fails, so we specify `fail-fast: false`.

![strategy-matrix](../../images/actions/strategy-matrix.png)

## Summary

In summary, the `strategy` and `matrix` keywords in GitHub Actions are powerful tools for implementing parallel workflows. These keywords allow you to define multiple combinations of inputs that should be run in parallel, greatly speeding up your workflow and increasing its reliability. By using them, you can easily implement parallel workflows in your own projects and take advantage of the many benefits that concurrency has to offer.

## Resources

- [**GitHub Actions - Strategy & Matrix**](https://christosgalano.github.io/github/github-actions-strategy-and-matrix/)
- [**Using a matrix for your jobs**](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs)
