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

        // CPU usage
        bat 'powershell -Command "Get-CimInstance Win32_Processor | Select-Object LoadPercentage"'

        // Memory usage
        bat 'powershell -Command "Get-CimInstance Win32_OperatingSystem | Select-Object FreePhysicalMemory,TotalVisibleMemorySize"'

        // Disk usage
        bat 'powershell -Command "Get-CimInstance Win32_LogicalDisk | Select-Object DeviceID,FreeSpace,Size"'
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

