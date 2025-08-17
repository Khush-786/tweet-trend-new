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
                sh 'mvn surefire-report:report'
                echo "------------unit test compleated-----------"
            }
        }

stage('SonarQube Analysis') {
    environment {
        scannerHome = tool 'valaxy-sonar-scanner'
    }
    steps {
        withSonarQubeEnv('valaxy-sonarqube-server') {
            withCredentials([string(credentialsId: 'sonar-cred', variable: 'SONAR_TOKEN')]) {
                sh ""
                    ${scannerHome}/bin/sonar-scanner \

                      //-Dsonar.projectKey=valaxy126-key_twittertrend \
                      //-Dsonar.sources=. \
                      //-Dsonar.java.binaries=target/classes \
                     //-Dsonar.token=${SONAR_TOKEN}
                ""
            }
        }
    }
}
    stage("Quality Gate"){
        steps {
            script {
  timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
    if (qg.status != 'OK') {
      error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
  }
}
        }
    }
}
}
