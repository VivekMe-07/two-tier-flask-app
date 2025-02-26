pipeline{
    agent {label "dev"};
    
    stages{
        stage("CODE"){
            steps{
                git url: "https://github.com/VivekMe-07/two-tier-flask-app.git", branch: "master"
                echo "Code has been deployed"
            }
        }
        stage("BUILD"){
            steps{
                sh "docker build -t my-app ."
                echo "Code has been build"
            }
        }
        
        stage("TEST"){
            steps{
                echo "Code has been Tested"
            }
        }
        
        stage("Push to Docker hub"){
            steps{
                withCredentials([usernamePassword(
                        credentialsId:"docker-cred",
                        passwordVariable:"dockerHubPass",
                        usernameVariable:"dockerHubUser"
                    )]){
                    
                echo "Pushing my image in docker hub"
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker image tag my-app ${env.dockerHubUser}/new-app:latest"
                sh "docker push  ${env.dockerHubUser}/new-app:latest"
                echo "My docker image has been pushed to docker hub"
                }
            }
        }
        
        stage("DEPLOY"){
            steps{
                sh "docker compose up -d --build flask-app"
                echo "Project has been deployed"
            }
        }
    }
}
