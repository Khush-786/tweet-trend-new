def registry = 'https://rishideshmukh9175.jfrog.io'
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
        

stage('SonarQube Analysis') {
    environment {
        scannerHome = tool 'valaxy-sonar-scanner'
    }
    steps {
        withSonarQubeEnv('valaxy-sonarqube-server') {
            withCredentials([string(credentialsId: 'sonar-cred', variable: 'SONAR_TOKEN')]) {
                sh """
                    ${scannerHome}/bin/sonar-scanner \
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
     stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"artfiact-cred"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
            }
        }   
    }   
}
    }



