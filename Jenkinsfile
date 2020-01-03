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
        stage('Test containers') {
            steps {
                //runs in the default containe (JNLP) 
                sh 'env'

                container('maven'){
                    sh 'env'
                    sh 'java -version'
                    sh 'mvn -version'
                }

                container('busybox') {
                    sh 'env'
                }
            }
        }
    }
}