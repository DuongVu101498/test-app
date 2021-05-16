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
                      withDockerRegistry(credentialsId: 'docker-hub-secret') {
                          docker_image.push("test-${env.BUILD_ID}")
                          docker_image.push('latest')
                      }
                    }
                }
              }
              stage('Staging deploy'){
                  sh ''' cat "k8s/staging-deploy.yaml" | sed "s/{{BUILD_ID}}/$BUILD_ID/g" | kubectl apply -f -
                         kubectl rollout status deployment.apps/netty
                     '''
              }
              # run on window
              stage('Load testing'){
                 agent {label 'window'}
                  steps {
                          build job: 'load-test'
                        }
                  post{
                      always{
                          node('linux'){
                              sh ''' kubectl delete service/netty deployment.apps/netty '''
                          }
                      }
                  }
              }
              stage('Staging deploy'){
                  sh ''' echo deploy'''
              }
           }
        }
    }
}

