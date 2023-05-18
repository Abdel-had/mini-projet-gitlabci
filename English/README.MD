# GitLab CI Mini-Project

![pipeline ci/cd](../images/pipeline-ci-cd.jpeg "pipeline ci/cd")

In this project, I containerized an existing static web application and set up a CI/CD pipeline on GitLab to automate the steps of building, testing, and deploying the application on Heroku. This README explains the pipeline stages, that you can find in the `.gitlab-ci.yml` file, and the advantages of this approach.

---

Author: Abdel-had HANAMI

Context: Bootcamp DevOps training, 12th promotion

Training center: eazytraining.fr

Period: March-April-May

Date: April 15, 2023

LinkedIn : https://www.linkedin.com/in/abdel-had-hanami/

## Overview of the CI/CD Pipeline

![pipeline ci/cd](../images/Gitlab-CI_pipeline.png "pipeline ci/cd")

## CI/CD Pipeline Workflow with Execution Conditions

1. **Build image**
   - *Condition*: Executes for each commit.
   - Builds the Docker image using the Dockerfile I created.
   - Uses a multi-stage approach to reduce the final image size and improve build times.
   
2. **Acceptance test**
   - *Condition*: Executes for each commit.
   - Runs acceptance tests on the built Docker image.
   - Loads the image from the `static-website.img.tar` artifact, then runs the container.
   - Tests the service by making an HTTP request and checking for specific content in the response.

3. **Release image**
   - *Condition*: Executes for each commit.
   - Publishes the Docker image to the GitLab registry.
   - Loads the image from the `static-website.img.tar` artifact, then tags it with the branch name and commit SHA.
   - Pushes the image to the GitLab registry to keep track of images for each commit and branch.

4. **Deploy review**
   - *Condition*: Executes for each new merge request or update to an existing merge request.
   - Deploys the application to a review environment on Heroku (for merge requests).
   
5. **Stop review**
   - *Condition*: Executes when a merge request is closed or accepted.
   - Stops the review environment on Heroku to free up resources and keep deployment environments clean.

6. **Deploy staging**
   - *Condition*: Executes when a commit is pushed to the `main` or `master` branch.
   - Deploys the application to a pre-production (staging) environment on Heroku.
   
7. **Test staging**
   - *Condition*: Executes after successful deployment to the pre-production environment.
   - Runs tests on the pre-production environment to ensure the application is working correctly.
   
8. **Deploy prod**
   - *Condition*: Executes manually after pre-production test validation.
   - Deploys the application to the production environment on Heroku.
   
9. **Test prod**
   - *Condition*: Executes after successful deployment to the production environment.
   - Runs tests on the production environment to ensure the application is functioning properly.


## Detailed Pipeline Explanation

### Build image

I created a Dockerfile to build a Docker image containing the static web application and the HTTP server. I used a multi-stage approach to reduce the final image size and improve build times. Advantages of this approach include a clear separation of concerns (cloning the repository, copying the source code) and reducing the final image size by including only the necessary files.

### Acceptance test

In this stage, acceptance tests are run on the built Docker image. The image is loaded from the artifact `static-website.img.tar`, then run as a container. The service is tested by performing an HTTP request and checking for specific content ("Dimension") in the response.

### Release image

Here, the Docker image is published to the GitLab registry. The image is first loaded from the artifact `static-website.img.tar`, then tagged with the branch name and commit SHA. It is then pushed to the registry. This allows keeping track of images for each commit and branch, making version tracking and deployment management easier.

### Deploy review, staging, and prod

I set up deployment stages for different environments (review, staging, and production) on Heroku. These stages ensure that the application is properly tested and verified before being deployed to production. The deployment stages use environment variables to differentiate environments and ensure the correct configurations are applied.

### Stop review

The "Stop review" stage is designed to stop and remove the review environment on Heroku when it is no longer needed. This frees up resources and keeps deployment environments clean.

### Test staging and Test prod

After each deployment to staging and production environments, I added stages to test the deployed application. This ensures the application is working correctly in the target environment before being considered ready for production use.

## Overview of the website


![webapp](../images/webapp.png "webapp")


## Technologies Used

- Docker: For containerizing the application and simplifying deployment.
- GitLab CI/CD: For automating the steps of building, testing, and deploying the application.
- Heroku: For hosting the application in different environments (review, staging, and production).

## Conclusion

By setting up this CI/CD pipeline for the static web application, I was able to automate the processes of building, testing, and deploying while ensuring the application is properly tested and functional before being put into production. Using Docker and GitLab CI/CD facilitated the management of environments and deployments, providing an efficient and reliable way to update and maintain the application.
