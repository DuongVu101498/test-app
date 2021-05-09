pipeline {
    agent none
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
                sh '''mvn --version
                      touch file1.txt
                      ls'''
                
            }
        }
        stage('window test') {
            agent { label 'window'}
            steps {
                bat 'mvn --version'
            }
        }
        stage('linux test') {
            agent { label 'linux'}
            steps {
                sh '''ls'''
            }
        }
    }
}
