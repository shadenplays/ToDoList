pipeline {
  agent any

    environment {
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
        bat """
                if not exist "${env.NPM_CONFIG_CACHE}" mkdir "${env.NPM_CONFIG_CACHE}"
                npm config set cache "${env.NPM_CONFIG_CACHE}"
                npm install
                """
            }
        }

        stage('Build') {
      steps {
        bat 'npx electron-vite build'
            }
        }

        stage('Test') {
      steps {
        echo "Running tests (none yet)..."
            }
        }

        stage('Archive Build') {
      steps {
        archiveArtifacts artifacts: 'out/**/*', allowEmptyArchive: false
            }
        }

        // ✅ Put it here, inside stages
        stage('Prod Server Instrumentation') {
      steps {
        echo "Collecting prod server stats..."
                bat 'wmic cpu get loadpercentage'
                bat 'wmic OS get FreePhysicalMemory,TotalVisibleMemorySize /Value'
                bat 'wmic logicaldisk get size,freespace,caption'
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
