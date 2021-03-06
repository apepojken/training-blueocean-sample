pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh './jenkins/build.sh'
        archiveArtifacts 'target/*.war'
      }
    }
    stage('Test') {
      steps {
        parallel(
          "Backend": {
            sh './jenkins/test-all.sh'
            junit '**/surefire-reports/**/*.xml'
            junit '**/test-results/karma/*.xml'
            
          },
          "Frontend": {
            sh './jenkins/test-frontend.sh'
            junit '**/test-results/karma/*.xml'
            
          },
          "Static": {
            sh './jenkins/test-static.sh'
            
          },
          "Performance": {
            sh './jenkins/test-performance.sh'
            
          }
        )
      }
    }
    stage('Deploy to staging') {
      steps {
        sh './jenkins/deploy.sh staging'
      }
    }
    stage('Deploy to Production') {
      steps {
        input(message: 'Deploy to production?', ok: 'Fire away!')
        sh './jenkins/deploy.sh production'
        sh 'echo "Notifying appropriate team members!"'
      }
    }
  }
}