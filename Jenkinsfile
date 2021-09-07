pipeline {
    agent {
        label 'master'
    }

    environment {
        PRODUCT = 'flask_api'
        registry = 'ashtonc96/flask_api'
        registryCredential = 'docker-creds'
    }

    stages {
        // Checkout the right branch
        stage("Env Build Number"){
            steps{
                echo "The build number is ${env.BUILD_NUMBER}"
            }
        }

        stage('Checkout') {
            steps {
                script {
                    git branch: 'main', credentialsId: 'git-creds', url: 'https://github.com/Ashtonc96/flask_api'
                }
            }
        }
        // Build a container with the code source of the application
        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":${env.BUILD_NUMBER}"
                }
            }
        }
        // Run the test using the built docker image - only for testing
        stage('Deploy') {
            steps {
                script {
                    sh "docker run --tty -p 5000:5000 ${env.registry}:${env.BUILD_NUMBER}"
                }
            }
        }
    }
}