pipeline {
    agent any

    environment {
        SONAR_HOST_URL = 'http://18.214.15.157:9000'
    }
    
    stages {
        stage("User") {
            steps {
                sh "whoami"
                sh "docker compose down || true" // don't fail if nothing to stop
            }
        }

        stage("Clone code") {
            steps {
                git url: "https://github.com/muhammadAbdullah4123/devops_project_01.git", branch: "main"
            }
        }

        stage('SonarQube Analysis') {
            steps {
                echo "Running SonarQube scan..."
                withSonarQubeEnv('SonarQubeServer') {
                    withCredentials([string(credentialsId: 'sonar_token', variable: 'SONAR_TOKEN')]) {
                        sh "mvn sonar:sonar \
                            -Dsonar.projectKey=project_01 \
                            -Dsonar.host.url=${SONAR_HOST_URL} \
                            -Dsonar.login=${SONAR_TOKEN}"
                    }
                }
            }
        }

        stage("Build image") {
            steps {
                sh "docker build -f Dockerfile --pull -t myimg:latest ."
            }
        }

        stage("Deploy image") {
            steps {
                sh "docker compose up -d --build"
            }
        }

        stage("Exit") {
            steps {
                echo "Every stage executed successfully..."
            }
        }
    }
}
