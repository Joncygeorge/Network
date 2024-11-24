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
                        url: 'https://github.com/TintuMariyamPaul/index.git'
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
                    if [ $(docker ps -aq -f name=html-sample) ]; then
                        docker stop html-sample
                        docker rm html-sample
                    fi
                    '''

                    // Run the new container with the latest image
                    sh 'docker run -d -p 3008:80 --name html-sample app:latest'
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
