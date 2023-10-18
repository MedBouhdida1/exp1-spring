pipeline {
    agent any 
    tools { 
        maven 'maven'
    }
    environment {
        SNAP_REPO = 'vprofile-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'bouzaien'
        RELEASE_REPO = 'maven-releases'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSIP = '44.201.187.193'
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
        NEXUSPASS = credentials('nexuspass')
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
        stage('Build') {
            steps {
                dir("exp1-spring"){
                      sh "mvn clean install"
                  }
            }
        }
        stage('UPLOAD ARTIFACT') {
                steps {
                    nexusArtifactUploader(
                        nexusVersion: 'nexus3',
                        protocol: 'http',
                        nexusUrl: "${NEXUSIP}:${NEXUSPORT}",
                        groupId: 'QA',
                        version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                        repository: "${RELEASE_REPO}",
                        credentialsId: ${NEXUS_LOGIN},
                        artifacts: [
                            [artifactId: 'vproapp' ,
                            classifier: '',
                            file: 'target/*.jar',
                            type: 'jar']
                        ]
                    )
                }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    dir("exp1-spring"){
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }

        
    }
}
