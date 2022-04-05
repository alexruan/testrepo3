pipeline {
  agent {
    label { 'node12' }
  }
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
        if (env.BRANCH_NAME.contains('master')){
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
      when {branch 'master'}
      agent {label 'docker'}
      steps {
        scripts {
          cleanws()
          sh "docker build -t angularhello/${env.BRANCH_NAME}"
        }
      }
    }
  }
}
