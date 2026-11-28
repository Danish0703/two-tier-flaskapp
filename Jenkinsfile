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
                sh "docker build -t two-tier-flaskapp ."
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DockerHubCreds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                       sh "docker login -u "$env.DOCKER_USER" -p "$env.DOCKER_PASS" "
                       sh "docker image tag two-tier-flaskapp "${env.DOCKER_USER}"/two-tier-flaskapp"
                       sh "docker push "$(env.DOCKER_USER)"/two-tier-flaskapp:latest"
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
