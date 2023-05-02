# GitHub Actions - Environments

## General

GitHub is a powerful tool for developers, and one of its most useful features is the ability to create different environments for different stages of development. In this post, we'll explore what GitHub environments are, how they work, and some examples of how they can be used.

An environment on GitHub is essentially a collection of resources and settings that are used to run code. These environments can be used for different stages of development, such as development, staging, and production. By creating separate environments for each stage, developers can ensure that their code is thoroughly tested and ready for deployment before it goes live.

One of the key benefits of using GitHub environments is that they allow for easy collaboration and testing. For example, a developer can create a development environment and invite other team members to collaborate on the code. This allows for real-time feedback and testing, which can help catch bugs and other issues early on.

Another benefit of GitHub environments is that they can be easily managed and configured. For example, developers can create a staging environment that mirrors the production environment, which allows for easy testing of new features and updates. Additionally, developers can use GitHub's built-in tools to manage and scale their environments as needed.

## Usage

So, how do you go about creating and using environments in GitHub?

First, you'll need to create a new repository on GitHub. Once the repository is created, you can create a new environment by going to the "Settings" tab and selecting "Environments." From there, you can create a new environment and configure it as needed.

Once your environment is set up, you can start deploying your code to it. You can do this by connecting your environment to a deployment tool such as GitHub Actions, CircleCI, or Travis CI. These tools allow you to automatically deploy your code to your environment whenever changes are made to your repository.

Another way to deploy code to environments is by using webhooks. A webhook is a way for GitHub to notify another service when certain events occur in a repository. For example, you could create a webhook that triggers a deployment to your environment whenever a pull request is merged.

Here are some examples of how you can use GitHub environments:

- **Development**: Create a development environment for each feature or bug fix that you're working on. This allows you to test your code changes in isolation and ensure that they don't break other parts of the application.

- **Staging**: Set up a staging environment that mirrors your production environment. This allows you to test new features and updates before they go live.

- **Production**: Use a production environment for your live application. This environment should be configured to handle high traffic and provide a stable and reliable experience for your users.

- **Continuous Integration and Deployment (CI/CD)**: Use GitHub Actions, CircleCI, or Travis CI to automatically deploy your code to your environment whenever changes are made. This allows you to quickly and easily deploy updates to your application without manual intervention.

- **A/B testing**: Create multiple versions of your application and deploy them to different environments. This allows you to test different variations and see which one performs best.

## Example

The `environment` keyword in GitHub Actions allows you to specify the environment to which your code should be deployed. This is particularly useful when you have multiple environments, such as development, staging, and production, and you want to ensure that your code is deployed to the correct environment.

The example we'll look at is available [**here**](https://github.com/christosgalano/GitHub-Actions-Deep-Dive/blob/main/.github/workflows/environments.yaml):

```yaml
name: environments

on:
  workflow_dispatch:

jobs:
  dev:
    runs-on: ubuntu-latest
    environment: development
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Deploy to development
        run: |
          echo "Deploying to ${{ vars.ENV}} with secret ${{ secrets.ADMIN_PASSWORD }} ..."
  prod:
    runs-on: ubuntu-latest
    environment: production
    steps:
      - name: Checkout
        uses: actions/checkout@v3
      - name: Deploy to production
        run: |
          echo "Deploying to ${{ vars.ENV}} with secret ${{ secrets.ADMIN_PASSWORD }} ..."
```

In this example, the workflow is triggered manually. We have two jobs which both check out the code and print a message.

The `environment` keyword is used to specify each job's environment.

The `vars` and `secrets` context is also showcased. These configurations can be set by going to "Settings/Environments" and selecting the environment you want to configure.

## Summary

In conclusion, GitHub environments are a powerful tool for developers that can help to improve collaboration, testing, and deployment. By creating separate environments for different stages of development, developers can ensure that their code is thoroughly tested and ready for deployment before it goes live. Additionally, GitHub environments can be easily managed and configured, making them a great choice for developers looking for an efficient way to handle different stages of their projects.

## Resources

- **Related repository:** [GitHub-Actions-Deep-Dive](https://github.com/christosgalano/GitHub-Actions-Deep-Dive)
- **Related documentation:** [Using environments for deployment](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment)
