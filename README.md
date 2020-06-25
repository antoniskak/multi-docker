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

## Production Overall Architecture
![Producation Overall](https://user-images.githubusercontent.com/36962615/85476145-85634080-b5af-11ea-88a3-8a3c23846309.png)

For this project we are using the Managed Data Service Providers **AWS Elastic Cache** and **AWS Relational Database(RDS)** to host the copies of Redis and Postgres which offer the following advantages:

**AWS ElastiCache**
1. Automatic creation and configuration of Redis instances
2. Scalability
3. Offers logging and maintenance
4. Security
5. Less dependency to Elastic Beanstalk (easier migration)

In order to create an AWS ElastiCache Cluster Instance for Redis we sign in to the AWS console and select **Services -> ElastiCache -> Redis -> Create** and continue with the appropriate configuration. We select a node type "cache.t2.micro" for this app and 0 Number of replicas.

**AWS Relational Database(RDS)**
1. Automatic creation and configuration of Postgres instances
2. Scalability
3. Offers logging and maintenance
4. Security
5. Less dependency to Elastic Beanstalk (easier migration)
6. Makes automatic backups of our database and rollbacks are super easy

In order to create an AWS RDS Database we log in to the AWS console and select **Services -> RDS -> Databases -> Create Database -> PostgreSQL** and continue with the appropriate configuration.

In order to enable communication between the Elastic Beanstalk and the external services (RDS and EC) we simply create a common Security Group for the instances inside our default Virtual Private Cloud (VPC) allowing any traffic among them.

We then add the Environment Variables to the Elastic Beanstalk Configuration by selecting **Environment -> Configuration -> Software -> Edit**.
**Environment properties**
<table>
  <tr>
    <td>REDIS_HOST</td>
    <td>[Redis Primary Endpoint] - removing the ":port" part</td>
  </tr>
    <tr>
    <td>REDIS_PORT</td>
    <td>6379</td>
  </tr>
    <tr>
    <td>PGUSER</td>
    <td>[Master Username]</td>
  </tr>
    <tr>
    <td>PGPASSWORD</td>
    <td>[Master password]</td>
  </tr>
    <tr>
    <td>PGHOST</td>
    <td>[DB Endpoint]</td>
  </tr>
    <tr>
    <td>PGDATABASE</td>
    <td>[DB cluster identifier]</td>
  </tr>
  <tr>
    <td>PGPORT</td>
    <td>5432</td>
  </tr>
</table>
as per our services' configuration.


### Deployment:
On **AWS Dashboard -> Services -> IAM -> Users** we create a new user with deploy access to Elastic Beanstalk (Programmatic Access and Attach existing Policies for Permissions). Then we set the **Access key ID** and **Secret access key** as Encrypted Environment Variables on Travis CI
<table>
  <tr>
    <td>AWS_ACCESS_KEY</td>
    <td>[Access key ID]</td>
  </tr>
    <tr>
    <td>AWS_SECRET_KEY</td>
    <td>[Secret access key]</td>
  </tr>
 </table>
