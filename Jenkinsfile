pipeline {
    agent {
        kubernetes {
            label 'my-pod-template'
            defaultContainer 'jnlp'
            yaml """
spec:
  containers:
  - name: maven
    image: maven:3.3.9-jdk-8-alpine
    command:
    - cat
    tty: true
    volumeMounts:
      - name: maven-cache
        mountPath: /root/.m2/repository
  - name: busybox
    image: busybox
    command:
    - cat
    tty: true
  volumes:
    - name: maven-cache
      hostPath:
        path: /tmp
        type: Directory
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