pipeline {
    agent {
        kubernetes {
            label 'my-pod-template'
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
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
                    stash(name: 'myStash', includes: 'target/**')
                }
            }
        }
        stage('test'){
            steps { 
                container('maven'){
                    unstash 'myStash'
                    sh './jenkins/test-backend.sh'
                    junit 'target/surefire-reports/**/TEST*.xml'
                }
            }
        }
    }
}