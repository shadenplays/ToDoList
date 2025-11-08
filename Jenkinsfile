pipeline {
  agent any

    environment {
    // Local npm cache to avoid permission issues
        NPM_CONFIG_CACHE = "${WORKSPACE}\\.npm_cache"
    }

    stages {
    // ------------------------
        stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/shadenplays/ToDoList.git'
            }
        }

        // ------------------------
        stage('Install Dependencies') {
      steps {
        // Ensure local npm cache exists and use it
                bat "if not exist \"${env.NPM_CONFIG_CACHE}\" mkdir \"${env.NPM_CONFIG_CACHE}\""
                bat "npm config set cache \"${env.NPM_CONFIG_CACHE}\""
                bat "npm install"
            }
        }

        // ------------------------
        stage('Build') {
      steps {
        bat "npx electron-vite build"
            }
        }

        // ------------------------
        stage('Test') {
      steps {
        echo "Running tests (none yet)..."
            }
        }

        // ------------------------
        stage('Archive Build') {
      steps {
        // Archive the build output; fail if nothing found
                archiveArtifacts artifacts: 'out/**/*', allowEmptyArchive: false
            }
        }

        // ------------------------
        stage('Prod Server Instrumentation') {
      steps {
        echo "Collecting prod server stats..."

                // CPU usage
                bat 'powershell -Command "Get-CimInstance Win32_Processor | Select-Object LoadPercentage | Out-String"'

                // Memory usage
                bat 'powershell -Command "Get-CimInstance Win32_OperatingSystem | Select-Object FreePhysicalMemory,TotalVisibleMemorySize | Out-String"'

                // Disk usage
                bat 'powershell -Command "Get-CimInstance Win32_LogicalDisk | Select-Object DeviceID,FreeSpace,Size | Out-String"'
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
