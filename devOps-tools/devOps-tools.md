# Streamlining Java Web Application Deployment with CI/CD

In today’s fast-paced development environment, automating the process of building, testing, and deploying applications is critical for improving efficiency and ensuring consistency. In this guide, I will walk you through the steps I took to implement a Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Java-based web application using Docker and GitHub Actions. From setting up the environment and creating the Dockerfile to automating the build process with GitHub Actions, I’ll share the entire process, along with key insights and best practices that streamline the deployment workflow. The repository used for this project is [DevOps-CI-CD-Pipeline-Project](https://github.com/Mohamedzonkol/DevOps-CI-CD-Pipeline-Project).

***

## Step 1: Cloning the Repository

I started by cloning the GitHub repository to my local machine. This repository contains a Java web application structured to be built with Maven.

### Command Used:
```bash
git clone https://github.com/Mohamedzonkol/DevOps-CI-CD-Pipeline-Project.git
```
***

## Step 2: Building the Java Web Application
The application is a Maven-based Java web application that outputs a .war file to be deployed on a Tomcat server.

### Steps to Build:
1. Navigated to the java-app/webapp directory.
2. Ran the Maven clean package command to generate the webapp.war file.
```bash
cd java-app/webapp
mvn clean package
```
***

## Step 3: Creating the Dockerfile
To containerize the web application, a Dockerfile was created. This defines the environment for running the application on a Tomcat server.

### Dockerfile Content:
```Dockerfile
FROM tomcat:9.0-jdk8-openjdk
COPY java-app/webapp/target/webapp.war /usr/local/tomcat/webapps/
```

### Explanation:
1. **Base Image**: Uses Tomcat 9 with JDK 8.
2. **Copy Command**: Places the webapp.war file in Tomcat’s webapps/ directory for automatic deployment.

***


## Step 4: Building the Docker Image
After creating the Dockerfile, the next step was to build the Docker image.

### Command Used:
```bash
docker build -t myapp .
```
This command uses the current directory to build the image and tags it as myapp.

***

## Step 5: Running the Docker Container
Once the Docker image was built, the next step was to run the Docker container. This step starts the Tomcat server, serving the .war file.

### Command Used:
```bash
docker run -p 8080:8080 myapp
```
This maps port 8080 of the container to port 8080 on the host, allowing us to access the application in the browser.

***

## Step 6: Setting Up CI/CD with GitHub Actions
DevOps practices revolve around automating processes such as building, testing, and deploying applications. For this project, GitHub Actions was used to automate the CI/CD pipeline.

### Workflow Configuration:
The following .github/workflows/ci-cd.yml file was created to automate the build and Docker image creation every time changes are pushed to the main branch:
```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Set up Java
        uses: actions/setup-java@v3
        with:
          java-version: 11

      - name: Build with Maven
        run: mvn clean package

      - name: Build Docker Image
        run: docker build -t myapp .
```

### Breakdown:
- **Trigger**: The pipeline is triggered every time there’s a push to the main branch.
- **Steps**:
  1. **Checkout Repository**: Retrieves the latest code from the repository.
  2. **Set up Java**: Sets up Java 11, which is required to build the Java application.
  3. **Build with Maven**: Runs Maven to clean and package the application, creating the .war file.
  4. **Build Docker Image**: Builds the Docker image based on the Dockerfile in the repository.

***

## Step 7: Running the GitHub Actions Workflow
After setting up the workflow file, every push to the main branch automatically triggers the pipeline. This ensures that the application is consistently built, packaged, and containerized without manual intervention.

***

## Conclusion
By integrating Docker and GitHub Actions, the project is now automated and ready for deployment at any time. This setup ensures that every change to the application is immediately built and containerized, improving the development lifecycle and reducing the risk of manual errors.

