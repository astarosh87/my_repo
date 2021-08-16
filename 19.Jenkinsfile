pipeline {
  agent {
    kubernetes {
        containerTemplate {
        name 'kubeval'
        image 'garethr/kubeval:latest'
        ttyEnabled true
        command 'watch date'
        }
    }
  }
  stages {
    stage('Clone repository') { 
        steps { 
                git url: 'https://github.com/astarosh87/my_repo.git'
        }
    }
    stage('Run kubeval in parallel') {
        parallel {
            stage('Check grafana.yaml') {
                steps {
                    container('kubeval') {
                      sh """#!/bin/sh
                        kubeval yamls/grafana.yaml
                        
                    """
                    }
                }
            }
            stage('Check prometheus.yaml') {
                steps {
                    container('kubeval') {
                      sh """#!/bin/sh
                        kubeval yamls/prometheus.yaml
                        
                    """
                    }
                }
            }
        }
    }
  }
  post {
        success {
                slackSend (color: 'good', message: "GOOD: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
            failure {
                slackSend (color: 'danger', message: "BAD: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
    }
}
