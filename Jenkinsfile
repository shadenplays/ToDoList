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

        // Use full PowerShell path
        bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -Command "Get-CimInstance Win32_Processor | Select-Object LoadPercentage"'
        bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -Command "Get-CimInstance Win32_OperatingSystem | Select-Object FreePhysicalMemory,TotalVisibleMemorySize"'
        bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -Command "Get-CimInstance Win32_LogicalDisk | Select-Object DeviceID,FreeSpace,Size"'
    }

    stage('Deploy to Prod') {
        steps {
          echo "Deploying app to production..."
        // Copy the latest build to a production folder
        bat """
        if not exist "C:\\prod_app" mkdir "C:\\prod_app"
        xcopy /Y /E "out\\*.*" "C:\\prod_app\\"
        """

        // Optional: launch the app (simulate production run)
        bat """
        start "" "C:\\prod_app\\ToDoList.exe"
        timeout /t 5
        """
    }
}

stage('Verify Deployment') {
        steps {
          echo "Verifying production app status..."
        bat 'tasklist | findstr /I "ToDoList.exe"'
    }
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

