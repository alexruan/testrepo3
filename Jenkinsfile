pipeline {
  agent any
  options {
    parallelsAlwaysFailFast()
    skipDefaultcheckout()
  }
  stages {
    stage('checkout') {
      steps {
        checkout scm
      }
    }
    stage('install'){
      steps {
        if (env.BRANCH_NAME.contains('main')){
          base = "origin/${env.BRANCH_NAME}" ;
        }
        sh "node -v && npm -v && npm ci"
      }
    }
    stage('app build'){
      steps {
        echo "running build"
        sh "ng build"
      }
    }
        	
    stage('docker'){
      when {branch 'main'}
      steps {
        scripts {
          cleanws()
          sh "docker build -t angularhello/${env.BRANCH_NAME}"
        }
      }
    }
  }
}
