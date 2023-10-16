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
        stage("onarQube Analysis") {
            def scannerHome = tool "sonarqub"';
              withSonarQubeEnv() {
                sh "${scannerHome}/bin/sonar-scanner"
              }
        }
    }
}
