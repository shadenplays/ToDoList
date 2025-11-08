pipeline {
  agent any

    stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/shadenplays/ToDoList.git'
            }
        }

        stage('Install Dependencies') {
      steps {
        bat '''
                if not exist ".npm_cache" mkdir ".npm_cache"
                npm config set cache ".npm_cache"
                npm install
                '''
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

                bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -Command "Get-CimInstance Win32_Processor | Select-Object LoadPercentage"'
                bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -Command "Get-CimInstance Win32_OperatingSystem | Select-Object FreePhysicalMemory,TotalVisibleMemorySize"'
                bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -Command "Get-CimInstance Win32_LogicalDisk | Select-Object DeviceID,FreeSpace,Size"'
            }
        }

        stage('Deploy to Prod') {
      steps {
        echo 'Deploying app to production...'

                // Copy build output to a "production" folder
                bat '''
                if not exist "C:\\prod_app" mkdir "C:\\prod_app"
                xcopy /Y /E "out\\*" "C:\\prod_app\\"
                '''

                // Optional: launch the built EXE to simulate deployment
                bat 'start "" "C:\\prod_app\\yourAppName.exe"'
            }
        }

        stage('Verify Deployment') {
      steps {
        echo 'Verifying production app...'
                bat 'tasklist | findstr /I "yourAppName.exe"'
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
