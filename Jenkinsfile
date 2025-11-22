
pipeline {
    agent any
    
    stages {
        stage("User"){
            steps{
                sh "whoami"
                sh "docker compose down"
            }
        }
        stage("Clone code"){
            steps{
                git url: "https://github.com/muhammadAbdullah4123/devops_project_01.git" , branch: "main"
            }
        }
        stage("Build image"){
            steps{
                sh "docker build -f Dockerfile --pull -t myimg:latest ."
            }
        }
        stage("Push image"){
            steps{
                withCredentials([usernamePassword(credentialsId:"jenkinsLogin", passwordVariable:"docpass", usernameVariable:"docuser")]){
                sh "docker login -u ${env.docuser} -p ${env.docpass}"
                sh "docker tag myimg:latest muhammadabdullah4123/project_01:myimg"
                sh "docker push muhammadabdullah4123/project_01:myimg"
                }
            }
        }
        stage("Deploy image"){
            steps{
                sh "docker compose up -d "
            }
        }
        stage("Exit"){
            steps{
                echo "Every stages executed successfully......"
            }
        }
    }
    
}
