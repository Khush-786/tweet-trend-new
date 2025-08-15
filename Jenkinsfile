pipeline {
    agent {
        node {
            label 'maven'
        }
    }

    tools {
        maven 'Maven-3.9.2'  
    }

    stages {
        stage("Build") {
            steps {
                sh 'mvn clean deploy'
            }
        }

stage('SonarQube Analysis') {
    tools { nodejs 'NodeJS_LTS' }
    environment {
        scannerHome = tool 'valaxy-sonar-scanner'
    }
    steps {
        withSonarQubeEnv('valaxy-sonarqube-server') {
            withCredentials([string(credentialsId: 'sonar-cred', variable: 'SONAR_TOKEN')]) {
                sh """
                    ${scannerHome}/bin/sonar-scanner \
                      -Dsonar.organization=valaxy126-key \
                      -Dsonar.projectKey=valaxy126-key_twittertrend \
                      -Dsonar.sources=. \
                      -Dsonar.java.binaries=target/classes \
                      -Dsonar.token=${SONAR_TOKEN}
                """
            }
        }
    }
}
}
}
