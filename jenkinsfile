pipeline{
    
    agent {
        node{
            label "Dev"
        }
    }
    
    stages{
        
        stage("Clone Code"){
            steps{
                git url: "https://github.com/pratiksha-design261/django-notes-app.git", branch: "main"
                echo "Code cloned"
            }
        }
        stage("Build & Test"){
            steps{
                sh "docker build . -t notes-app:latest"
                echo "Built successfully, and tested without error"
            }
        }
        stage("Push Image to Docker"){
            steps{
                withCredentials(
                    [usernamePassword(
                        credentialsId:"DockerHub",
                        passwordVariable:"dockerHubPass",
                        usernameVariable:"dockerhubUser"
                        )
                    ]
                ){
                sh "docker image tag notes-app:latest ${env.dockerhubUser}/notes-app:latest"
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerhubUser}/notes-app:latest"
                }
                echo "Image pushed successfully"
            }
        
        }
        stage("deploy"){
            steps{
                //sh "docker run -d -p 8000:8000 --name Django-note-app notes-app:latest"
                sh "docker compose down"
                sh "docker compose up -d"
                echo "DEployment is done"
            }
        }
        
    }
}
