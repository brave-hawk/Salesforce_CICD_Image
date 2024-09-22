# Salesforce_CICD_Image
A personalized salesforce container repo for CI/CD

Contains: 
 - salesforce/cli:latest-full
 - salesforce/cli:latest-slim

Additional packages installed:
- sfdx-git-delta
- sfdx-scanner
- pip
- yq

## Steps to reproduce
I used the following steps to create this setup

### Create docker hub account
Head to hub.docker.com and create an account. 
I used login with github for this.

### Generate github token
Next, head back to your github account.
- Click on your user profile and select settings.
- On the left sidebar, select Developer settings 
- Click Personal access tokens, and generate a classic token. Call it: **GHCR_DOCKER_IMAGE_LOGIN**
- Scope: write-package, delete-package
- **N/B** keep this token somewhere secure till end of the steps, as you will not be able to read it again from github *(1)**

### Create github repo to hold your Dockerfiles and workflow
This repo will hold the Docker file(s) that will be used to build the image

We shall use a folder structure that holds the full and slim versions of salesforce/cli 
 (refer to this repo: Salesforce_CICD_Image)

- In VSCode, init a repo and create the files and folders as in this repo, then push
- Navigate back to github > your repo > Settings > Secrets and variables > Actions
- Create a new repository secret.
- For name, I wrote **DOCKER_GHCR_TOKEN**
- For the secret, paste in the token you kept somewhere secure: *see note (1)**
  
  We shall use this to authenticate to docker and publish and access our image from the Github Container Registry (GHCR)

### Create the dockerfiles
Create the docker files as seen in the repo. 

Here we install any extra software that we need and is not in the official Salesforce image

We also create an entrypoint bash script to display our cool ascii art as the splash screen

### Create the github workflow
This will allow us to build and publish our image as a package in our repo

We have configured the workflow each time we push to the branch, so that we don't have to manually build the 
image every time.

The **publish_image** yaml file is pretty self-explanatory. I've included comments to show what each step does

Once you commit, the worflow should run, and you should see a new package in your github account.

That's pretty much it.

### Bonus
To build and test the docker image locally (I'm using Ubuntu):

- Install docker as specified here: https://docs.docker.com/engine/install/ubuntu/

To run in an interactive shell (without build), simply do:
```
docker run -it salesforce/cli:latest-slim
```
This allows you to interact with the container and test-run install packages

#### To build and publish the image locally:
```
docker login --username [github-username] --password [github-secret-in-note] ghcr.io
```
This specifies to docker that we intend to use the github registry, and not the default dockerhub registry

You may have a warning telling you not to paste in github keys in stdin


To build:
```
docker build -t ghcr.io/brave-hawk/salesforce_ci-cd:latest-slim .
```
Replace *brave-hawk* with your github username and *salesforce_ci-cd* with your repo name and *latest* with a tag of your choice


To list docker images
```
docker images
```
You should now see your newly built image here
ex: *ghcr.io/brave-hawk/salesforce_ci-cd* with tag: *latest-slim*


To publish to your image to GHCR:
```
 docker push ghcr.io/brave-hawk/salesforce_ci-cd:latest-slim
```










