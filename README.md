# CI/CD pipeline

this repository serves as a pipeline automating deployments of projects to my website, [rbk6.dev](https://rbk6.dev).

## deploy-website

this workflow handles the deployment of my [website repository](https://github.com/rbk6/website) with the following flow:

1. trigger on commit to master branch:
   a. checkout the repository within the github runner
   b. build the docker image
   c. (todo) run tests within the container
   d. push the image to docker hub
2. deploy to virtual private server:
   a. ssh into the vps hosting my website
   b. pull the latest image from docker hub
   c. build the image with the `production` parameter to clean and copy the files to the apache directory, replacing the previous version
3. final cleanup:
   a. cleanup any leftover docker image(s) within the github runner

### rollback

currently, there is no rollback method in place. if a failure occurs, the workflow will fail before reaching the point of SSHing into the VPS and updating the existing code, so no changes will be made.
