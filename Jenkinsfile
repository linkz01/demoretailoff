pipeline {
  agent any
  tools { maven 'Maven3' }

  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/linkz01/demoretailoff.git', branch: 'main'
      }
    }
    stage('Code Analysis') {
      steps {
        bat 'mvn -B -q checkstyle:check spotbugs:check'
      }
    }
    stage('Build & Test') {
      steps {
        bat 'mvn -B clean verify'
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          archiveArtifacts artifacts: 'target/*.jar', fingerprint: true, onlyIfSuccessful: true
        }
      }
    }
    stage('Deploy (simulado)') {
      steps {
        bat '''
          if not exist C:\\jenkins-deploy mkdir C:\\jenkins-deploy
          copy /Y target\\*.jar C:\\jenkins-deploy\\
        '''
        echo 'Artefacto copiado a C:\\jenkins-deploy (deploy simulado).'
      }
    }
  }
}
