pipeline {
    agent any

    stages {
        stage("Clone Code") {
            steps {
                echo "Cloning the Code"
                git url: "https://github.com/ShehrozeYousuf/django-notes-app.git" , branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building the image"
		sh "docker build -t my-note-app ."
            }
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image to Docker hub"
		withCredentials([usernamePassword(credentialsId:"DockerHub",passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]){
		sh "docker tag my-note-app ${env.dockerHubUser}/my-note-app:latest"
		sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
		sh "docker push ${env.dockerHubUser}/my-note-app:latest"
		}
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the Container"
		sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
