pipeline {
  agent any

    environment {
    // Optional: Set npm cache to workspace
        NPM_CONFIG_CACHE = "${env.WORKSPACE}\\.npm_cache"
    }

    stages {
    stage('Checkout SCM') {
      steps {
        checkout scm
            }
        }

        stage('Install Dependencies') {
      steps {
        // Ensure npm cache directory exists
                bat """
                if not exist "%NPM_CONFIG_CACHE%" mkdir "%NPM_CONFIG_CACHE%"
                npm config set cache "%NPM_CONFIG_CACHE%"
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
        echo 'Running tests (none yet)...'
                // Add test commands here if needed, e.g. `npm test`
            }
        }

        stage('Archive Build') {
      steps {
        archiveArtifacts artifacts: 'out/**/*', allowEmptyArchive: true
            }
        }

        stage('Prod Server Instrumentation') {
      steps {
        echo 'Collecting prod server stats...'
                // CPU load
                bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -Command "Get-CimInstance Win32_Processor | Select-Object LoadPercentage"'
                // Memory
                bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -Command "Get-CimInstance Win32_OperatingSystem | Select-Object FreePhysicalMemory,TotalVisibleMemorySize"'
                // Disk space
                bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -Command "Get-CimInstance Win32_LogicalDisk | Select-Object DeviceID,FreeSpace,Size"'
            }
        }
    }

    post {
    success {
      echo '✔ Build successful!'
        }
        failure {
      echo '❌ Build failed!'
        }
    }
}
