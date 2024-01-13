Overview

This repository contains a Jenkins pipeline script for continuous integration and deployment (CI/CD) of a Java project. The pipeline is designed to build the project using Maven, perform SonarQube static analysis, build a Docker image, and push it to Docker Hub with dynamic versioning based on Git commit and timestamp.
Prerequisites

    Jenkins installed and configured.
    Maven tool configured in Jenkins global tool configuration.
    JDK 17 tool configured in Jenkins global tool configuration.
    Docker installed on the Jenkins server.
    SonarQube server set up (if static analysis is enabled).

Jenkins Pipeline Overview

The Jenkins pipeline is structured as follows:

    Agent and Options:
        The pipeline is configured to run on any available agent.
        Concurrent builds are disabled to avoid race conditions.

    Tools:
        Maven version 'M3' and JDK version 'Java-17' are configured.

    Environment:
        Environment variables are defined, including GIT_REPO_NAME, GIT_COMMIT, TIME_STAMP, and DOCKERHUB_CREDENTIALS.

    Stages:

        Build Stage:
            Maven is used to build the project.

        Scan Docker Image Stage:
            SonarQube analysis is performed on the project.

        Build Image Stage:
            A Docker image is built with a dynamic tag based on repository name, commit hash, and timestamp.

        Push Image Stage:
            Docker Hub credentials are used to log in, and the Docker image is pushed to Docker Hub.

    Functions:
        determineRepoName(): Extracts the repository name from the GIT_URL.
        getCommit(): Retrieves the first 8 characters of the GIT_COMMIT.
        buildVersion(): Generates a build version timestamp.

Usage

    Ensure Jenkins is set up and configured with the required tools and plugins.
    Create a Jenkins pipeline job and point it to this repository.
    Configure the Docker Hub credentials in Jenkins (named 'ercydann-dockerhub' in the script).
    Run the Jenkins pipeline job.

Notes

    The pipeline skips tests during the build (mvn -B -DskipTests clean package).
    Ensure that Docker is properly configured on the Jenkins server for image building and pushing.