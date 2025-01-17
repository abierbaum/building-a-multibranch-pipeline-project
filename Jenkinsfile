pipeline {
   agent {
      docker {
         image 'node:6-alpine'
         args '-p 3000:3000 -p 5000:5000'
         label 'master'
      }
   }
   environment {
      CI = 'true'
   }
   stages {
      stage('Build') {
         agent {
            docker {
               image 'node:6-alpine'
               args '-p 3000:3000 -p 5000:5000'
               label 'linux'
            }
         }
         steps {
            echo 'starting'
            sh 'pwd'
            sh 'npm install'
         }
      }
      stage('Test') {
         agent {
            docker {
               image 'node:6-alpine'
               args '-p 3000:3000 -p 5000:5000'
               label 'master'
            }
         }
         steps {
            sh 'pwd'
            sh './jenkins/scripts/test.sh'
         }
      }

      stage('Deliver for development') {
         when {
            branch 'development'
         }
         steps {
               sh './jenkins/scripts/deliver-for-development.sh'
               input message: 'Finished using the web site? (Click "Proceed" to continue)'
               sh './jenkins/scripts/kill.sh'
         }
      }
      stage('Deploy for production') {
         when {
               branch 'production'
         }
         steps {
               sh './jenkins/scripts/deploy-for-production.sh'
               input message: 'Finished using the web site? (Click "Proceed" to continue)'
               sh './jenkins/scripts/kill.sh'
         }
      }
   }
}
