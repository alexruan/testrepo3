#!groovy
pipeline {
  agent any
  options {
    parallelsAlwaysFailFast()
    skipDefaultCheckout()
  }
  stages {
    stage('checkout') {
      when { branch 'multi' }
      steps {
        checkout scm
        milestone(1)
      }
    }
    stage('install'){
      when { branch 'multi' }
      steps {
        script {
          if (env.BRANCH_NAME.contains('multi')){
            base = "origin/${env.BRANCH_NAME}"
            echo "${base}"
          }
          echo "${env.BRANCH_NAME}"
          sh "node -v && npm -v && npm ci"
          milestone(2)
        }
      }
    }
    stage('app build'){
      when { branch 'multi' }
      steps {
        script {
          echo "running build"
          sh "ng build"
          milestone(3)
        }
      }
    }
        	
    stage('docker'){
      when { branch 'multi' }
      steps {
        script {
          imageName="angularhello/${env.BRANCH_NAME}"
          imageTag = env.BRANCH_NAME.replaceAll('/','-')
          sh "docker build -t ${imageName} ."
          cleanWs()
          milestone(4)
        }
      }
    }
  }

  post {
    always {
      echo "Job: ${env.JOB_NAME}"
      echo "Status:  ${currentBuild.currentResult}"
      echo "Build number ${env.BUILD_NUMBER} "
    }
  }
}
