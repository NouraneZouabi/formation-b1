pipeline {
    agent any

    tools {
        nodejs 'node'
        maven 'maven'
    }
    stages {
        stage("Clean up "){
            steps {
                deleteDir()
            }
        }

        stage ("clonage du repo "){
            steps{
                sh "git clone https://github.com/NouraneZouabi/formation-b1.git"
            }
        }
        stage ("Création de l'image frontend"){
            steps{
                dir("formation-b1/angular-app"){
                    sh "docker build -t nouran10/myapp-frontend:latest . --no-cache"
                    sh "docker push nouran10/myapp-frontend:latest"
                }
            }
        }

        stage ("Création de l'image backend"){
            steps{
                dir("formation-b1/springboot/app"){
                    sh "mvn clean install"
                    sh "docker build -t nouran10/myapp-backend:latest . --no-cache"
                    sh "docker push nouran10/myapp-backend:latest"
                }
            }
        }

    }
}