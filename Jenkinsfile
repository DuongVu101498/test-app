pipeline {
    agent none
    stages{
        stage('run on linux'){
            agent {label 'linux'}
            stages {
              stage('Build') {
                agent {
                    docker {
                       image 'maven:3.8.1-adoptopenjdk-15'
                       args '-v $HOME/.m2:/root/.m2'
                       reuseNode true
                    }
                 }
                steps {
                     sh '''mvn clean package
                           '''
                      }
              }
             stage("Build Docker image") {
                steps {
                  script {
                    docker_image = docker.build("duongvt16/netty-app:${env.BUILD_ID}")
                  }
                }
             }

             stage("Push images") {
                steps {
                   script {
               
                      withDockerRegistry(credentialsId: 'docker-hub-secret', url: "https://hub.docker.com") {
                          docker_image.push("test")
                          docker_image.push(${env.BUILD_ID})
                   }
                }
              }
              stage('linux test') {
               steps {
                   sh ''' ls
                          ls -a $HOME/.m2
                          '''
                     }
              }
           }
        }
    }
}
}
