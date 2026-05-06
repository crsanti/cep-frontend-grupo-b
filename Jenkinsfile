pipeline {
  agent any

  stages {
    stage('Initial') {
      steps {
        sh 'ls -alh'
        sh 'node --version'
        sh 'npm --version'
      }
    }

    stage('Install') {
      steps {
        sh 'npm install'
      }
    }

    stage('Format check') {
      steps {
        warnError(message: 'La guía de estilo no se ha respetado') {
          sh 'npm run format:check'
        }
        script {
          if (currentBuild.result == 'UNSTABLE') {
            currentBuild.description = 'UNSTABLE: Format Check'
          }
        }
      }
    }

    stage('Lint') {
      steps {
        sh 'npm run lint'
      }
    }

    stage('Type check') {
      steps {
        sh 'npm run type-check'
      }
    }

    stage('Tests') {
      steps {
        sh 'npm run test'
      }
    }

    stage('Build') {
      steps {
        sh 'npm run build'
        sh 'zip -r dist.zip dist'
        archiveArtifacts(artifacts: 'dist.zip', fingerprint: true)
      }
    }
  }

  post {
    always {
      cleanWs()
    }

    success {
      echo 'Pipeline completed successfully'
    }

    unstable {
      echo 'Pipeline pass with some errors.'
    }

    failure {
      echo 'Pipeline failed. Review logs.'
    }


  }
}
