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
            environment {
                scannerHome = tool 'valaxy-sonar-scanner'
            }
            steps {
                withSonarQubeEnv('valaxy-sonarqube-server') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}
