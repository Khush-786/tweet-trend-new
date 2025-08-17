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
                echo "------------build started------------------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "------------build compleated ------------"
            }
        }
        stage("test"){
            steps{
                echo "------------unit test started--------------"
                sh 'mvn surefile-report:report'
                echo "------------unit test compleated-----------"
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
