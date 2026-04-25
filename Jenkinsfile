pipeline {
    agent any

    tools {
        nodejs 'node'
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
                dir("app/angular-app"){
                    sh "docker build -t nouran10/myapp-frontend:latest . --no-cache"
                    sh "docker push nouran10/myapp-frontend:latest"
                }
            }
        }

    }
}