# Description
Example of building a simple fibonacci calculator with an over-complicated and complex architecture, using multi containers,
establishing communication between the different containers and services, designing a Continuous Integration Workflow for Multiple
Images syncing the github repo with Travis CI and setting up the multi-container deployments to AWS Elastic Beanstalk.

## Architecture Overview
### Development:
![DevelopmentDiagram](https://user-images.githubusercontent.com/36962615/85207349-1cb26480-b320-11ea-8971-296d972d45b7.png)

### Application Workflow Diagram
![Application Architecture](https://user-images.githubusercontent.com/36962615/85206323-284e5d00-b319-11ea-90e8-1c043c25cfc4.png)

## Routing with Nginx
Nginx will be watching for incoming requests and routing them to the appropriate backend server (servers that Nginx proxies requests to are known as upstream servers) depending on a set of routing rules.
![Nginx Routing](https://user-images.githubusercontent.com/36962615/85209563-ef21e700-b330-11ea-8bfa-8538ae02cd7e.png)

## Development Build
**docker-compose.yml** configures the dev environment and ```docker-compose up``` starts the app on  http://localhost:3050/.

### Production:
![Production Diagram](https://user-images.githubusercontent.com/36962615/85331527-3a253100-b4ce-11ea-8203-43b393deb357.png)

### Production Deployment Flow:
Travis CI will automatically pull the repo from github. Every new push will trigger a build of a test image from travis, which will test the code and then a build of the production images. The test image is discarded after testing.

Production images are then pushed to Docker Hub by Travis CI.

Travis CI notifies Elastic Beanstalk about the project deployment.

Elastic Beanstalk pulls those images and builds the containers for the new deployment out of them.
