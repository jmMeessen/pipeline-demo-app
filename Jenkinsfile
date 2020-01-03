pipeline {
    agent {
        kubernetes {
            label 'my-pod-template'
            defaultContainer 'jnlp'
            yaml """
spec:
  containers:
  - name: maven
    image: maven:alpine
    command:
    - cat
    tty: true
  - name: busybox
    image: busybox
    command:
    - cat
    tty: true
"""
        }
    }
    stages {
        stage('build') {
            steps {
                container('maven') {
                    sh './jenkins/build.sh'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('test'){
            steps { 
                container('maven'){
                    sh './jenkins/test-backend.sh'
                    junit 'target/surefire-reports/**/TEST*.xml'
                }
                sh 'env'
                container('busybox') {
                    sh 'env'
                }
            }
        }
    }
}