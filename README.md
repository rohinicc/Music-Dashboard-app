🎵 Music Dashboard App
A music dashboard web application built with HTML, containerized using Docker, and deployed through a Jenkins CI/CD pipeline with DockerHub integration.

Tech Stack

HTML — Frontend UI
Docker — Containerization
Jenkins — CI/CD pipeline
DockerHub — Docker image registry
GitHub — Source code management


Project Structure
Music-Dashboard-app/
├── index.html        # Main dashboard UI
├── Dockerfile        # Docker image configuration
├── Jenkinsfile       # Jenkins pipeline definition
└── .dockerignore     # Files excluded from Docker build

Jenkins Pipeline Stages
GitHub Push
    ↓
1. Check Docker Version
    ↓
2. Build Docker Image
    ↓
3. Tag Image (build ID + latest)
    ↓
4. List Docker Images
    ↓
5. Remove Old Container
    ↓
6. Run New Container (port 8085)
    ↓
7. Push Image to DockerHub ✅
Stage Details
StageDescriptiondocker versionVerifies Docker is installed on the slave nodebuild docker imageBuilds image tagged as musicappdocker tagTags image with build ID and latestdocker imageLists all available Docker imagesRemove old containerRemoves existing container named app before redeploymentRun docker containerRuns new container on port 8085Push to DockerHubPushes both versioned and latest tags to DockerHub

Environment Variables
VariableValueDescriptionDOCKER_IMAGEmusicappLocal image nameDOCKERHUB_USERNAMErohiniccDockerHub usernameDOCKERHUB_REPOmusic-dashboard-appDockerHub repository nameVERSION$BUILD_IDJenkins build ID used as image tagCONTAINER_NAMEappRunning container nameCONTAINER_PORT8085Host portREQUEST_PORT80Container portDOCKERHUB_CREDdockerhub-credJenkins credentials ID for DockerHub

Jenkins Setup Requirements

Jenkins slave node labeled slave_node with Docker installed
DockerHub credentials saved in Jenkins as dockerhub-cred
Docker permissions configured for Jenkins user (sudo docker)

Add DockerHub Credentials in Jenkins

Go to Jenkins → Manage Jenkins → Credentials
Click Add Credentials
Kind: Username and Password
ID: dockerhub-cred
Enter DockerHub username and password


DockerHub Image
bash# Pull latest image
docker pull rohinicc/music-dashboard-app:latest

# Run the container
docker run -d -p 8085:80 rohinicc/music-dashboard-app:latest

# Open in browser
http://localhost:8085

Access the App
Once the pipeline runs successfully:
http://<server-ip>:8085

Post Actions
StageConditionActionRemove old containersuccessLogs: Old container removedRemove old containerfailureLogs: Container not present, runs new oneRun docker containeralwaysLogs: Container is runningRun docker containersuccessLogs: Container running successfullyRun docker containerfailureLogs: Failed to run container
