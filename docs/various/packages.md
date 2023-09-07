# GitHub Packages

> **Simplifying package management**

## Overview

In the world of software development, efficient package management is crucial for smooth collaboration, version control, and seamless deployment of projects. GitHub recognized this need and introduced GitHub Packages.

## What is GitHub Packages?

GitHub Packages is a package management service provided by GitHub. It allows developers to publish and consume software packages, including libraries, frameworks, and other dependencies, within the GitHub ecosystem. By leveraging the power of package registries, GitHub Packages simplifies the process of sharing, versioning, and distributing code across multiple projects and organizations.

## How does it work?

GitHub Packages integrates seamlessly with your existing GitHub repositories, enabling you to manage your project's dependencies more efficiently. It supports multiple package formats, including:

- npm - JavaScript
- gem - Ruby
- mvn - Java
- gradle - Java
- nuget - .NET
- Docker containers

Each package format has its own unique workflows, but the underlying principles remain consistent.

To get started with GitHub Packages, you can either publish your own packages or consume packages published by others. Publishing packages involves configuring your repository's package settings, such as specifying the package name, version, and dependencies. Once published, these packages become available to other developers.

When consuming packages, you can add them as dependencies to your projects. GitHub Packages provides a package registry URL and authentication mechanism, allowing your build systems or package managers to retrieve the required dependencies during the build process. This ensures that your projects always use the correct and consistent versions of the packages.

## Benefits

- **Centralized package management:** GitHub Packages consolidates package management within your existing GitHub workflow, reducing the need to rely on external package registries or repositories.
- **Version control integration:** By hosting packages on GitHub, you can take full advantage of version control features, such as tagging, branching, and pull requests, ensuring better traceability and collaboration.
- **Access control and security:** GitHub Packages leverages the access control mechanisms of GitHub repositories, enabling you to define who can publish and consume packages. It also ensures secure and authenticated access to your packages.
- **Seamless integration:** GitHub Packages seamlessly integrates with popular package managers like npm, Maven, and RubyGems, making it easy to incorporate into existing workflows without major disruptions.
- **Visibility:** With GitHub's vast community, packages published on GitHub Packages gain visibility, making it easier for developers to discover and reuse packages developed by others.

## Example of GitHub Packages in action

Let's take a look at the ci/cd workflow used in [**devops-with-github-example**](https://github.com/christosgalano/devops-with-github-example):

```yaml  
publish-docker-image:
  name: publish-docker-image
  needs: test
  runs-on: ubuntu-latest
  permissions:
    contents: read
    packages: write
  steps:
    - name: Checkout repository
      uses: actions/checkout@v4
    
    - name: Log in to the Container registry
      uses: docker/login-action@v2
      with:
        registry: ${{ env.REGISTRY }}
        username: ${{ github.repository_owner }}
        password: ${{ secrets.GITHUB_TOKEN }}
    
    - name: Build and push Docker image
      id: build-push-image
      uses: docker/build-push-action@v4
      with:
        context: ${{ github.workspace }}/src
        push: true
        tags: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}
  
deploy-application:
  name: deploy-application
  needs: publish-docker-image
  runs-on: ubuntu-latest
  environment:
    url: https://${{ vars.WEBAPP_NAME }}.azurewebsites.net
    name: production-application
  permissions:
    contents: read
  steps:
    - name: Checkout repository
      uses: actions/checkout@v4  
    
    - name: Azure login
      uses: Azure/login@v1
      with:
        creds: ${{ secrets.AZURE_CREDENTIALS }}      
    
    - name: Configure webapp
      run: |
        az webapp config container set \
        --name ${{ vars.WEBAPP_NAME }} \
        --resource-group ${{ vars.RESOURCE_GROUP_NAME }} \
        --docker-custom-image-name '${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}:${{ github.sha }}' \
        --docker-registry-server-url ${{ env.REGISTRY }} \
        --docker-registry-server-user ${{ github.repository_owner }} \
        --docker-registry-server-password ${{ secrets.PACKAGES_PAT }}
```

We can see that the workflow consists of two jobs: `publish-docker-image` and `deploy-application`. The first job builds and pushes a Docker image to the GitHub Packages registry. The second job deploys the application to Azure, using the Docker image published in the previous step.

Here is also the published package in GitHub:

![devops-with-github-package](/assets/images/various/devops-with-github-package.png)

## Summary

GitHub Packages provides a powerful and convenient package management solution that integrates seamlessly with your GitHub repositories. It streamlines the process of sharing, versioning, and distributing software packages, enhancing collaboration and ensuring reliable and consistent builds. By leveraging GitHub Packages, developers can focus on building innovative solutions while benefiting from the vast ecosystem of packages within the GitHub community.

## References

- [**GitHub Packages**](https://docs.github.com/en/packages)
