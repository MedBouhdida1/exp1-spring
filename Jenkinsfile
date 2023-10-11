pipeline {
    agent any 
    tools { 
        maven 'maven'
    }
    stages {
        stage ("Clean up"){
            steps {
                deleteDir()
            }
        }
        stage ("Clone repo"){
            steps {
                sh "git clone https://github.com/MaBouz/exp1-spring.git"
            }
        }
        stage ("Generate backend image") {
              steps {
                   dir("exp1-spring"){
                      sh "mvn clean install"
                      sh "sudo docker build -t docexp1-spring ."
                  }                
              }
          }
        stage ("push image to docker hub") {
              steps {
                   dir("exp1-spring"){
                      sh "docker tag docexp1-spring malbouz/docexp1-spring:1.0.0"
                      sh "docker push malbouz/docexp1-spring:1.0.0"
                  }                
              }
          }
        stage ("Run docker compose") {
            steps {
                 dir("exp1-spring"){
                    sh " docker compose up -d"
                }                
            }
        }
    }
}
