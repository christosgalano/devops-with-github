# GitHub Actions: Actions

> **Creating your own actions**

## Overview

GitHub Actions is a powerful automation tool that allows developers to create custom workflows for their projects. These workflows, or "actions", can be used to automate a wide variety of tasks, such as building and testing code, deploying code to production, and more. In this blog post, we will take a detailed look at what actions are and how to create your own using an example of a custom action that i've written and use regularly. The name of the action is `create-update-label` and its purpose is to update an issue label (or create one if one does not exist) in every repository of a user.

## What are GitHub Actions?

In GitHub, actions are individual, reusable pieces of code that can be run in a workflow. These actions can be written in a variety of programming languages, such as JavaScript, Python, and more. Each action has a specific purpose, such as building code, running tests, or deploying code to a production environment.

Actions can be built and shared by the community, and also can be built by yourself. To create your own action, you will first need to create a new repository in GitHub. This repository will contain the code for your action, as well as any necessary configuration files. Once your repository is created, you can start writing your action.

You can build Docker containers, JavaScript, and composite actions. Actions require a metadata file to define the inputs, outputs, and main entrypoint for your action. The metadata filename must be either `action.yml` or `action.yaml`.

## Components of an Action

There are two main components to an action: the action.yaml file and the code file. The action.yaml file is a configuration file that contains information about the action, such as its name, inputs, and outputs. The code file is where the actual code for the action is located.

Here's the action.yaml for `create-update-label`:

```yaml
name: "Create/Update Label"
author: "Christos Galanopoulos"
description: "Create or update a label in all of the user's repositories"
inputs:
  name:
    description: "Specify label name"
    required: true
  color:
    description: "Specify label color"
    required: true
  description:
    description: "Specify label description"
    required: true
  token:
    description: "Specify GitHub Personal Access Token"
    required: true
branding:
  icon: edit-2
  color: gray-dark
runs:
  using: "composite"
  steps:
    - shell: bash
      run: |
        ${{ github.action_path }}/create_update_label.sh \
          -n ${{ inputs.name }} \
          -c ${{ inputs.color }} \
          -d ${{ inputs.description }} \
          -a ${{ inputs.token }}
```

In this example, the action is named "Create/Update Label" and it has four inputs:

- `name`: which is the label's name
- `color`: which is the color that is going to be used for the label
- `description`: which is the label's description
- `token`: which is a GitHub Personal Access Token with the necessary permissions (specifically you need **Read access to code and metadata** and **Read and Write access to issues** on a scope that includes all repositories to which you want to add the label)

The action is `composite` which allows you to package multiple actions together into a single action, making it easy to reuse and share those actions across multiple workflows.

The code for this action is located in the `create_update_label.sh` file, which looks something like this:

```bash
# Create a label
function create_label() {
    curl -s \
    -X POST \
    -H "Accept: application/vnd.github+json" \
    -H "Authorization: Bearer $api_token" \
    -H "X-GitHub-Api-Version: 2022-11-28" \
    https://api.github.com/repos/$repo/labels \
    -d "{\"name\": \"$name\", \"description\": \"$description\" , \"color\": \"$color\"}"
}

# Update a label, if it does not exist create it
function update_label() {
    status_code=$(curl -s \
        -X PATCH \
        -H "Accept: application/vnd.github+json" \
        -H "Authorization: Bearer $api_token" \
        -H "X-GitHub-Api-Version: 2022-11-28" \
        https://api.github.com/repos/$repo/labels/$name \
        -d "{\"name\": \"$name\", \"description\": \"$description\" , \"color\": \"$color\"}" \
        --write-out '%{http_code}' \
    --output /dev/null)
    if [ "$status_code" -eq 404 ]; then
        create_label
        printf "Successfully created $name label in $repo\n"
    else
        printf "Successfully updated $name label in $repo\n"
    fi
}

# Parse and validate parameters
parse_params "$@"
validate_params

# Fetch all repositories and keep only their full names (user/repo)
repos=$(curl -s \
    -H "Accept: application/vnd.github+json" \
    -H "Authorization: Bearer $api_token" \
    -H "X-GitHub-Api-Version: 2022-11-28" \
https://api.github.com/user/repos | jq -r '.[].full_name')

# Update the specified label in every repository.
# If it does not exist then create it.
for repo in $repos
do
    update_label
done
```

The whole script can be found [**here**](https://github.com/christosgalano/Workflows-Actions-Library/blob/main/.github/actions/create-update-label/create_update_label.sh).

## Using Actions in a Workflow

Once you have created your action, you can use it in a workflow. As you have learned by now, a workflow is a series of actions that are run in a specific order. Here's an example of a workflow that uses the `create-update-label`:

```yaml
name: create-update-label
on:
  push:
    branches:
    - main

jobs:
  create-update-label:
    runs-on: ubuntu-latest
    steps:
      - uses: christosgalano/Workflows-Actions-Library/.github/actions/create-update-label@main
        with:
          name: demo
          color: 0075ca  # in hex, without the '#'
          description: "A demo label"
          token: ${{ secrets.GITHUB_PAT }}
```

In this example, the workflow is named "create-update-label" and it is triggered by a push to the main branch. The workflow contains a single job called "create-update-label" that runs on the latest version of Ubuntu. The job has only one step:

- Use the `create-update-label` action

## Summary

To summarize, GitHub Actions allows users to create and reuse their own actions in their workflows so that they can automate repetitive tasks and save time. They can be written in any language and run on any operating system, and they can be used to perform a variety of tasks such as building, deploying code, running tests, and sending notifications. They can be stored in a separate repository, shared and reused across multiple projects and teams, and also versioned to ensure they are tested and working correctly before they are used in production.

## References

- [**About custom actions**](https://docs.github.com/en/actions/creating-actions/about-custom-actions)
