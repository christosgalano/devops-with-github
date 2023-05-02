# GitHub Actions: Secrets

## Overview

GitHub Secrets is a powerful feature that allows you to store and manage sensitive data, such as API keys, tokens, and passwords, in a secure and encrypted manner. By using GitHub Secrets, you can keep your sensitive data safe and secure, while still being able to use it in your workflows.

When you create a secret in GitHub, it is encrypted and stored securely on GitHub's servers. You can then reference a secret in a workflow by using the GitHub Secrets context. This allows you to use sensitive data, such as an API key, in a secure manner without having to hardcode it into your workflow or store it in plain text.

To create a secret in GitHub, you can go to the **Settings** of your repository and navigate to the **Secrets and variables** tab. From there, you can create a new secret by providing a name and the value of the secret. It's important to note that once you create a secret, **you cannot view or edit the value again**.

It's also worth noting that GitHub Secrets can also be used to store sensitive data for use in GitHub Actions workflows that run on GitHub-hosted runners. This means that you can store and use secrets in your workflows without having to worry about them being exposed in plain text.

Another important aspect of GitHub Secrets is that they are scoped to a repository, so they can only be accessed by workflows that run in the same repository. This means that if you have multiple workflows that need to access the same secret, you don't have to create a separate secret for each workflow.

## Example

The example we'll look at is available [**here**](https://github.com/christosgalano/GitHub-Actions-Deep-Dive/blob/main/.github/workflows/secrets.yaml):

```yaml
name: azure-account-show
on: push

jobs:
  azure-account-show:
    runs-on: ubuntu-latest
    steps:
      - name: Azure login
        uses: azure/login@v1
        with:
          creds: ${{ secrets.AZURE_CREDENTIALS }}
    
      - name: Show account information
        uses: azure/CLI@v1
        with:
          inlineScript: |
            az account show
```

In this example, the workflow is triggered by a push to the repository. The azure-account-show job includes two steps. The first one uses an action called [`azure/login@v1`](https://github.com/marketplace/actions/azure-login) to log into Azure using the value of the secret `AZURE_CREDENTIALS`. The second step simply prints information related to the identity we have logged in as.

## Summary

In summary, GitHub Secrets is a powerful feature that allows you to store and manage sensitive data, such as API keys, tokens, and passwords, in a secure and encrypted manner. By using GitHub Secrets, you can keep your sensitive data safe and secure, while still being able to use it in your workflows. By referencing a secret in a workflow, you can use sensitive data in a secure manner without having to hardcode it into your workflow or store it in plain text. With GitHub Secrets, you can automate your software development process without compromising security.

## References

- [**Encrypted secrets**](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
