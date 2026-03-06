pipeline {
    agent any
    tools { 
        maven 'maven'
        nodejs 'node'
    }
    stages {
        stage ("Clean up"){
            steps {
                deleteDir()
            }
        }
        stage ("Clone repo"){
            steps {
                bat "git clone https://github.com/NouraneZouabi/Dep.git"
            }
        }
        stage ("Generate frontend image") {
            steps {
                 dir("app/angular-app"){
                    bat "docker build -t nouran10/myapp-frontend . --no-cache"
                    bat "docker push nouran10/myapp-frontend"
                }                
            }
        }
        stage ("Generate backend image") {
              steps {
                   dir("app/springboot/app"){
                      bat "mvn clean install"
                      bat "docker build -t nouran10/myapp-backend . --no-cache"
                      bat "docker push nouran10/myapp-backend"
                  }                
              }
          }
        stage ("Run docker compose") {
            steps {
                 dir("app"){
                    bat "docker compose down --volumes" 
                    bat "docker compose pull"
                    bat "docker compose up -d"
                }                
            }
        }
    }
}






