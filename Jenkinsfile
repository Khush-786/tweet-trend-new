pipeline {
    agent {
        node {
          label 'maven'
        }
    }

    stages {
        stage("Build"){
            step{
                  sh 'mvn clean Deploy'
            }
        }
        stage {
            environment {
                scannerHome = tool 'valaxy-sonar-scanner'
            }
            steps{
                withSonarQubeEnv('valaxy-sonarqube-server'){
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
    }
}