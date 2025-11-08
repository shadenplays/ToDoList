pipeline {
  agent any

    environment {
    NODE_ENV = 'production'
        // Cache folder for node_modules
        NPM_CACHE = "${env.WORKSPACE}\\.npm_cache"
    }

    options {
    // Keep only the last 10 builds
        buildDiscarder(logRotator(numToKeepStr: '10'))
        // Show timestamps in console
        timestamps()
    }

    stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/shadenplays/ToDoList.git'
            }
        }

        stage('Install Dependencies') {
      steps {
        script {
          // Create npm cache folder if not exists
                    bat """
                        if not exist "%NPM_CACHE%" mkdir "%NPM_CACHE%"
                        npm config set cache "%NPM_CACHE%"
                        npm install
                    """
                }
            }
        }

        stage('Build') {
      steps {
        script {
          // Use npx to ensure local electron-builder is found
                    bat 'npm electron-vite build'
                }
            }
        }

        stage('Test') {
      steps {
        echo 'Running tests (none yet)...'
                // Example: bat 'npm run test'
            }
        }

        stage('Archive Build') {
      steps {
        archiveArtifacts artifacts: 'out/**/*', allowEmptyArchive: false
            }
        }
    }

    post {
    success {
      echo '✅ Build successful!'
        }
        failure {
      echo '❌ Build failed!'
        }
    }
}
