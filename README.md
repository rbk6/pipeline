# CI/CD pipeline

this repository serves as a pipeline automating deployments of projects to my website, [rbk6.dev](https://rbk6.dev).

## deploy-website

this workflow handles the deployment of my [website repository](https://github.com/rbk6/website).

### setup

in order to setup the workflow, the following variables will need to be set within [github secrets](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions), specifically the repository ones:

- **DEPLOY_TOKEN**: github fine-grained PAT with read/write permission for actions/contents
- **DOC_ROOT**: apache document root
- **DOCKER_USERNAME**
- **DOCKER_PASSWORD**
- **VPS_HOST**
- **VPS_USER**
- **VPS_SSH_KEY**
- **VPS_SSH_PORT**

### workflow

1. trigger on commit to master branch:
   1. checkout the repository within the github runner
   1. build the docker image
   1. (todo) run tests within the container
   1. push the image to docker hub
2. deploy to virtual private server:
   1. ssh into the vps hosting my website
   1. pull the latest image from docker hub
   1. build the image with the `production` and `$DOC_ROOT` parameters to clean and copy the files to the apache directory, replacing the previous version
3. final cleanup:
   1. cleanup any leftover docker image(s) within the github runner

### rollback

currently, there is no rollback method in place. if a failure occurs, the workflow will fail before reaching the point of SSHing into the VPS and updating the existing code, so no changes will be made.
