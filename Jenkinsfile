#!groovy
pipeline {
  agent any
  options {
    parallelsAlwaysFailFast()
    skipDefaultCheckout()
  }
  stages {
    stage('checkout') {
      steps {
        checkout scm
      }
    }
    stage('install'){
      steps {
        script {
          echo "${env.BRANCH_NAME}"
          sh "node -v && npm -v && npm ci"
        }
      }
    }
    stage('app build'){
      steps {
        script {
          echo "running build"
          sh "ng build"
        }
      }
    }
        	
    stage('docker'){
      steps {
        script {
          sh "docker build -t angularhello/main ."
          cleanws()
        }
      }
    }
  }
}
