pipeline {
    agent any

    tools { 
        maven 'maven'
        nodejs 'node'
    }

    stages {

        stage("Clean up"){
            steps {
                deleteDir()
            }
        }

        stage("Clone repo"){
            steps {
                bat "git clone https://github.com/NouraneZouabi/Dep.git"
            }
        }

        stage("Generate frontend image") {
            steps {
                dir("Dep/angular-app"){
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS')]) {

                        bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"
                        bat "docker build -t nouran10/myapp-frontend . --no-cache"
                        bat "docker push nouran10/myapp-frontend"
                    }
                }
            }
        }

        stage("sonar test") {
          steps {
            dir("Dep/springboot/app") {
                bat "set MAVEN_USER_HOME=C:\\Jenkins\\.m2&& mvnw.cmd clean install"
                bat """
                  mvnw.cmd clean verify sonar:sonar ^
                   -Dsonar.projectKey=deploy-app ^
                   -Dsonar.host.url=http://54.196.35.185:9000 ^
                   -Dsonar.login=sqp_6f462fb68efb239e5a5d404a1dc7044c4abd916e
               """
                 }
          }
     }
        
        stage("Generate backend image") {
            steps {
                dir("Dep/springboot/app"){
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-creds',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS')]) {

                        bat "echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin"

                        bat "set MAVEN_USER_HOME=C:\\Jenkins\\.m2&& mvnw.cmd clean install"
                        
                        bat "docker build -t nouran10/myapp-backend . --no-cache"
                        bat "docker push nouran10/myapp-backend"
                    }
                }
            }
        }

        stage("Deploy") {
            steps {
                dir('Dep/') {
                    withKubeConfig([credentialsId: 'kubeconfig', serverUrl: 'https://54.196.35.185:6443']) {
                        bat 'kubectl config view'
                        bat 'kubectl get nodes'
                        powershell 'kubectl apply -f k8s/*'
                        bat 'kubectl apply -f ingress.yaml'
                    }
                }
        }
    }
}
}





















