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
