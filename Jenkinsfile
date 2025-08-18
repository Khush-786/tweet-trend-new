deregistry = 'https://rishideshmukh9175.jfrog.io'
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
<<<<<<< HEAD
     stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:"https://rishideshmukh9175.jfrog.io/artifactory" ,  credentialsId:"artifact-cred"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "target/*.jar",
                              "target": "libs-release-local/com/valaxy/demo-workshop/${env.BUILD_NUMBER}/",
                              "flat": "true",
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



=======
    
  }
}
        
>>>>>>> f9a7829 (added jenkinsfile)
