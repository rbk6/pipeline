# CI/CD pipeline

this repository serves as a pipeline automating deployments of projects to my website, [rbk6.dev](https://rbk6.dev).

## deploy-website

this workflow handles the deployment of my [website repository](https://github.com/rbk6/website).

### setup

in order to setup the workflow, the following variables will need to be set within [github secrets](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions), specifically the repository ones:

- **DEPLOY_TOKEN**: github fine-grained PAT with read/write permission for actions/contents
- **VPS_HOST**
- **VPS_USER**
- **VPS_SSH_KEY**
- **VPS_SSH_PORT**

### workflow

the deploy-website workflow is triggered by updates to the website repository and performs the following steps:

1. checkout from website repository
1. build docker image and run tests
1. ssh to vps
1. update local rebuild image
1. copy from container
1. restart the service

### rollback

currently, there is no rollback method in place. if a failure occurs, the workflow will fail before reaching the point of SSHing into the VPS and updating the existing code, so no changes will be made.
