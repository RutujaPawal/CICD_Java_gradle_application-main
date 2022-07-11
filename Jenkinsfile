pipeline{
    agent any
    environment{
        VERSION = "${env.BUILD_ID}"
    }
    stages{
        stage("sonar quality check"){
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        sh 'chmod +x gradlew'
                        sh './gradlew sonarqube'
                   }
                }
            }
       }
        stage("docker build & docker push"){
            steps{
                script{
                    withCredentials([string(credentialsId: 'docker_pass', variable: 'docker_password')]) {
                             sh '''
                                docker build -t 34.122.66.28:8083/springapp:${VERSION} .
                                docker login -u rutuja123hanmant -p $docker_password 34.122.66.28:8083 
                                docker push  34.122.66.28:8083/springapp:${VERSION}
                                docker rmi 34.122.66.28:8083/springapp:${VERSION}
                            '''
                    }
                }
            }
        }
   }
}

