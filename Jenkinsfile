
pipeline {
    agent {label: ubuntu}

    environment {
        // SonarQube configuration
        SONAR_HOST_URL = 'http://18.214.15.157:9000'
        SONAR_SCANNER = 'SonarQubeScanner' // Jenkins configured SonarQube scanner tool
    }
    
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

        tage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube scan..."
                withSonarQubeEnv('SonarQubeServer') { // 'SonarQubeServer' is the name configured in Jenkins
                    sh "mvn sonar:sonar \
                        -Dsonar.projectKey=project_01 \
                        -Dsonar.host.url=${SONAR_HOST_URL} \
                        -Dsonar.login=${sqa_0ebb15611f224cbe5e0e21e37d2a56bf90ab9eda}"
                }
            }
            
        stage("Build image"){
            steps{
                sh "docker build -f Dockerfile --pull -t myimg:latest ."
            }
        }
       
        stage("Deploy image"){
            steps{
                sh "docker compose up -d --build"
            }
        }
        stage("Exit"){
            steps{
                echo "Every stages executed successfully......"
            }
        }
    }
    
}
