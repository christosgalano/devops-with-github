# Branch Deployment Models

> **Deploy with confidence**

## Overview

Whether you're a software or devops engineer, you're probably in scenarios where you need to deploy your code on a daily basis. It makes no difference whether the code is infrastructure or application-related; the necessity to deploy it in a secure and reliable manner is always present. As a result, it is critical to understand the main deployment models accessible to you and select the one that best fits your needs.

## Merge deploy model

The classic merge deploy model consists of the following steps:

1. Create a new branch
2. Update the newly created branch
3. Open a pull request
4. Check whether the proposed changes are acceptable
5. Merge the pull request into the main branch
6. Deploy the main branch

![merge-deploy-model](/assets/images/various/merge-deploy-model.jpeg)

## Branch deploy model

The branch deploy model consists of the following steps:

1. Create a new branch
2. Update the newly created branch
3. Open a pull request
4. Check whether the proposed changes are acceptable
5. Deploy the branch
6. Check whether the deployed changes are acceptable
7. Merge the pull request into the main branch

![branch-deploy-model](/assets/images/various/branch-deploy-model.jpeg)

## Comparison

The merge deploy model carries inherent risks as the main branch is never consistently stable. When facing deployment failures or the need for rollbacks, the entire process must be repeated to revert the changes.

On the other hand, the branch deploy model ensures that the main branch remains in a "desirable" state, allowing for seamless deployment rollbacks if needed. In this model, changes are merged into the main branch only after successful deployment and validation of the branch. This approach minimizes risks and ensures a more reliable deployment process.

## Principles of the branch deploy model

- The main branch is regarded as a stable and deployable branch at all times. It represents the production-ready state of the codebase.

- All changes undergo deployment to the production environment before they are merged into the main branch. This ensures that modifications are thoroughly tested and validated in a real-world scenario.

- In the event of a need to roll back a branch deployment, the main branch serves as the key mechanism. By deploying the main branch, you can revert to a known stable state and effectively undo the changes introduced by the branch deployment.

The branch deployment model emphasizes maintaining a reliable main branch and prioritizes the verification of changes in a production environment before merging them. This approach enables greater control and confidence in the deployment process while providing a clear path for rollback if required.

## Conclusion

The branch deploy model is a more reliable and secure deployment model than the merge deploy model. It ensures that the main branch is always in a stable state and that changes are thoroughly tested and validated before being merged. This approach also provides a clear path for rollback if needed.

## References

- [**Enabling branch deployments through IssueOps with GitHub Actions**](https://github.blog/2023-02-02-enabling-branch-deployments-through-issueops-with-github-actions/)
- [**terraform-template-repo**](https://github.com/christosgalano/terraform-template-repo)
