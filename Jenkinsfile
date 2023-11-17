pipeline {

    agent any
    //disable concurrent build to avoid race conditions and to save resources
    options {
        disableConcurrentBuilds()
    }

    //load tools - these should be configured in jenkins global tool configuration
     tools {
        maven 'M3'
        jdk 'Java-17'
    }
    environment {
            GIT_REPO_NAME = determineRepoName()
    }
    stages {


        stage('Build') {
            steps {
                 sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                //
            }
        }

        // Security Scan  - SAST
        // Security scan - SCA


        stage('Build Image') {
            steps {
                //
            }
        }
        //TODO add a security scan for the image Trivy
        stage('Scan Docker Image') {
            steps {
                //
            }
        }

        stage('Push Image') {
            steps {
                //
            }
        }
    }
}