pipeline {
  agent any

    environment {
    NODE_ENV = 'production'
    }

    stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/shadenplays/ToDoList.git'
            }
        }

        stage('Install Dependencies') {
      steps {
        bat 'npm install'
            }
        }

        stage('Build') {
      steps {
        bat 'npm run build'
            }
        }

        stage('Test') {
      steps {
        echo 'Running tests (none yet)'
                // Add actual test commands here later, e.g.:
                // bat 'npm run test'
            }
        }

        stage('Archive Build') {
      steps {
        // Archive the correct Electron/Vite output folder
                archiveArtifacts artifacts: 'out/**/*', allowEmptyArchive: false
            }
        }
    }

    post {
    success {
      echo 'Build completed successfully!'
        }
        failure {
      echo 'Build failed!'
        }
    }
}
