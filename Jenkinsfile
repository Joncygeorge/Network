pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                checkout scmGit(
                    branches: [[name: '*/main']],
                    extensions: [],
                    userRemoteConfigs: [[
                        credentialsId: 'githubtoken',
                        url: 'https://github.com/Joncygeorge/Network.git'
                    ]]
                )
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t app:latest .'
                }
            }
        }
        stage('Deploy Docker Container') {
            steps {
                script {
                    // Stop and remove the existing container if it exists
                    sh '''
                    if [ $(docker ps -aq -f name=html-application) ]; then
                        docker stop html-application
                        docker rm html-application
                    fi
                    '''

                    // Run the new container with the latest image
                    sh 'docker run -d -p 3008:80 --name html-application app:latest'
                }
            }
        }
    }

    post {
        always {
            script {
                // Optional cleanup of old unused images
                sh 'docker image prune -f'
            }
        }
    }
}
