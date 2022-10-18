# github-actions-template
A repository for github actions template

---

## Workflows

### CI-service
The CI service workflow jobs:<br>
1. run unit tests <br>
2. build and push docker images with relavant tags to an image registry<br>
3. create a helm chart with the updated image tags and upload it to an S3 bucket<br>

inputs:<br>
dockerfile_path - path to the dockerfile<br>
context_path - path to the context(docker build start location)<br>
service_name - the name of the service being built<br>

The pipeline will run unit tests, followed by building and pushing the services image to an ECR, if the specified ECR does not exist it will create it.<br>
The image tags that will be pushed are: run_id, branch_name, the github commit SHA.<br><br>
Once the image has been succefully pushed, the updated helm chart will be published to an S3 bucket:<br>
A new version.yaml file will be created with the latest tags of the triggering branch for all services, if the no tag is found for the branch name the "dev" branch will be used as the tag.
This file is created inside the main umbrella folder, the whole helm chart is zipped and published to an S3 bucket.<br>
if the S3 bucket does no exist, it will be created by the workflow.
