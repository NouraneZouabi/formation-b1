pipeline {
    agent any 
    tools { 
        maven 'maven'
        nodejs 'nodejs'
    }
    stages {
        stage ("Clean up"){
            steps {
                deleteDir()
            }
        }
        stage ("Clone repo"){
            steps {
                sh "git clone https://github.com/NouraneZouabi/Dep.git"
            }
        }
        stage ("Generate frontend image") {
            steps {
                 dir("app/angular-app"){
                    sh "docker build -t nouran10/myapp-frontend . --no-cache"
                    sh "docker push nouran10/myapp-frontend"
                }                
            }
        }
        stage ("Generate backend image") {
              steps {
                   dir("app/springboot/app"){
                      sh "mvn clean install"
                      sh "docker build -t nouran10/myapp-backend . --no-cache"
                      sh "docker push nouran10/myapp-backend"
                  }                
              }
          }
        stage ("Run docker compose") {
            steps {
                 dir("app"){
                    sh "docker compose down --volumes" 
                    sh "docker compose pull"
                    sh "docker compose up -d"
                }                
            }
        }
    }
}

