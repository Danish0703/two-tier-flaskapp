pipeline{
    
    agent { label "agent-1"};
    
    stages{
        stage("Code Clone"){
            steps{
               script{
                   clone("https://github.com/Danish0703/two-tier-flaskapp.git", "master")
               }
            }
        }
        
        stage("Build"){
            steps{
                sh "docker build -t two-tier-flask-app ."
            }
            
        }
       
        stage("Push to Docker Hub"){
            steps{
                script{
                    docker_push("dockerHubCreds","two-tier-flask-app")
                }  
            }
        }
        stage("Deploy"){
            steps{
                sh "docker compose up -d --build flask-app"
            }
        }
    }

