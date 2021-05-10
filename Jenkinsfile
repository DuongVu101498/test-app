pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                     image 'maven:3.8.1-adoptopenjdk-15'
                     args '-v /home/duong/.m2:/root/.m2'
                     reuseNode true
                }
            }
            steps {
                sh '''mvn clean package
                      '''
                
            }
        }
        stage('linux test') {
            agent { label 'linux'}
            steps {
                sh ''' ls
                       ls -a target
                       ls -a /home/duong
                       ls -a /home/duong/.m2'''
            }
        }
    }
}
