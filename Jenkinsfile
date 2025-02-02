pipeline {
    agent any
    
    stages {
        stage("Code Clone") {
            steps {
                echo "Cloning the code..."
                //git url:"https://github.com/shivampawar183/django-notes-app.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building the image..."
                sh "docker build -t todo-note-app:v1 ."
            }
        }
        stage("Push") {
            steps {
                echo "Pushing the image into dockerHub..."
                withCredentials([usernamePassword(credentialsId:"dockerHub", usernameVariable:"dockerHubUser", passwordVariable:"dockerHubPass")]) {
                sh "docker tag todo-note-app:v1 ${env.dockerHubUser}/todo-note-app:v1"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker push ${env.dockerHubUser}/todo-note-app:v1"
                }
            }
        }
        stage ("Deploy") {
            steps {
                echo "Deploying the app into the server..."
                withCredentials([usernamePassword(credentialsId:"dockerHub", usernameVariable:"dockerHubUser", passwordVariable:"dockerHubPas")]) {
                sh "docker run -d -p 8000:8000 ${env.dockerHubUser}/todo-note-app:v1"
                // sh "docker compose down && docker compose up -d"
                }
            }
        }
    }
}
