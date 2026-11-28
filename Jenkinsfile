pipeline {
    agent { label 'agent-1' }

    stages {
        stage('Code Clone') {
            steps {
                git branch: 'master', url: 'https://github.com/Danish0703/two-tier-flaskapp.git'
            }
        }

        stage('Build') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${IMAGE_TAG} ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DockerHubCreds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${DOCKER_IMAGE}:${IMAGE_TAG}
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh "docker compose up -d --build"
            }
        }
    }
}
