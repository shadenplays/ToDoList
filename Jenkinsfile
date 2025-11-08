pipeline {
  agent any

    environment {
    // Use a local npm cache to avoid permission issues on Windows Jenkins
        NPM_CONFIG_CACHE = "${WORKSPACE}\\.npm_cache"
    }

    stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/shadenplays/ToDoList.git'
            }
        }

        stage('Install Dependencies') {
      steps {
        // Create local npm cache folder and install dependencies
                bat """
                    if not exist "${env.NPM_CONFIG_CACHE}" mkdir "${env.NPM_CONFIG_CACHE}"
                    npm config set cache "${env.NPM_CONFIG_CACHE}"
                    npm install
                """
            }
        }

        stage('Build') {
      steps {
        // Build Electron Vite app
                bat 'npx electron-vite build'
            }
        }

        stage('Test') {
      steps {
        echo "Running tests (none yet)..."
                // Add future test commands here
            }
        }

        stage('Archive Build') {
      steps {
        // Archive all build outputs from 'out' folder
                archiveArtifacts artifacts: 'out/**/*', allowEmptyArchive: false
            }
        }
    }

    post {
    success {
      echo "✔ Build successful!"
        }
        failure {
      echo "❌ Build failed!"
        }
    }
}
