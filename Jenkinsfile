pipeline {
    agent any

    options {
        // Disable concurrent builds to avoid race conditions and save resources
        disableConcurrentBuilds()
    }

    // Load tools configured in Jenkins global tool configuration
    tools {
        maven 'M3'
        jdk 'Java-17'
    }

    environment {
        // Define environment variables
        GIT_REPO_NAME = determineRepoName()
        GIT_COMMIT = getCommit()
        TIME_STAMP = buildVersion()
        DOCKERHUB_CREDENTIALS = credentials('ercydann-dockerhub')
    }

    stages {
        stage('Build') {
            steps {
                // Build the project using Maven
                sh 'mvn -B -DskipTests clean package'
            }
        }

        stage('Scan Docker Image') {
            steps {
                // Run SonarQube analysis
                withSonarQubeEnv(installationName: 'sonar') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

        stage('Build Image') {
            steps {
                // Build Docker image with a unique tag(DYNAMIC IMAGE TAGGING)
                sh 'docker build -t ${GIT_REPO_NAME}:dev-${GIT_COMMIT}-${TIME_STAMP} .'
            }
        }

        stage('Push Image') {
            steps {
                script {
                    // Log in to Docker Hub
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'

                    // Push the Docker image to Docker Hub
                    sh "docker push ${GIT_REPO_NAME}:dev-${GIT_COMMIT}-${TIME_STAMP}"

                    // Log out from Docker Hub
                    sh "docker logout"
                }
            }
        }
    }
}

// Function to determine the repository name from the GIT_URL
String determineRepoName() {
    return GIT_URL.replaceFirst(/^.*\/([^\/]+?).git$/, '$1')
}

// Function to get the first 10 characters of the GIT_COMMIT
String getCommit() {
    return GIT_COMMIT[0..9]
}

// Function to generate a build version timestamp
def buildVersion() {
    timestamp = Calendar.getInstance().getTime().format('YYYYMMddHHmmss', TimeZone.getTimeZone('EAT'))
    return timestamp
}

// Function to get the project version from the Maven POM file
def version() {
    pom = readMavenPom file: 'pom.xml'
    return pom.version
}
