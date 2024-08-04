pipeline{
    
    agent any;

    stages{
        stage("Code"){
            steps{
                git url: "https://github.com/dheeruyadav54/django-notes-app.git", branch: "main"
                echo "Clone the Code from github repo"
            }
        }
        stage("Build"){
            steps{
                echo "Build the Docker image"
                sh "whoami"
                sh "docker build -t notes-app-jenkins:latest ."
                echo "making this typo deliberately for pipeline to fail"
        }
    }
        stage("Push to DockerHub"){
            steps{
                echo "Install the Docker Compose package into Jenkins Agent Machine"
                echo "Push the image to DockerHub"
                withCredentials(
                [usernamePassword(
                    credentialsId: "dockerHubCreds",
                    passwordVariable: "dockerHubpass",
                    usernameVariable: "dockerHubuser"
                    )
                ]
            ){
                sh "docker image tag notes-app-jenkins:latest ${env.dockerHubuser}/notes-app-jenkins:latest"
                sh "docker login -u ${env.dockerHubuser} -p ${env.dockerHubpass}"
                sh "docker push ${env.dockerHubuser}/notes-app-jenkins:latest"
            }
        }
    }
        stage("Deploy"){
            steps{
                sh "docker compose up -d"
                echo "Docker Container has been built successfully"
            }
        }
    }
}
