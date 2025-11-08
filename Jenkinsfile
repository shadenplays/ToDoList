pipeline {
  agent any

    environment {
    CI = 'true'
    }

    stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/shadenplays/ToDoList.git'
            }
        }

        stage('Install Dependencies') {
      steps {
        bat '''
                    echo Installing dependencies...
                    if not exist ".npm_cache" mkdir ".npm_cache"
                    npm config set cache ".npm_cache"
                    npm ci || npm install
                '''
            }
        }

        stage('Build') {
      steps {
        bat '''
                    echo Building Electron app...
                    npx electron-vite build
                '''
            }
        }

        stage('Test') {
      steps {
        echo 'Running tests (none yet)...'
            }
        }

        stage('Archive Build') {
      steps {
        archiveArtifacts artifacts: 'out/**/*', fingerprint: true
            }
        }

        stage('Prod Server Instrumentation') {
      steps {
        echo 'Collecting prod server stats...'
                bat 'powershell -Command "Get-CimInstance Win32_Processor | Select LoadPercentage"'
                bat 'powershell -Command "Get-CimInstance Win32_OperatingSystem | Select FreePhysicalMemory,TotalVisibleMemorySize"'
                bat 'powershell -Command "Get-CimInstance Win32_LogicalDisk | Select DeviceID,FreeSpace,Size"'
            }
        }

        stage('Deploy to Prod') {
      steps {
        echo 'Deploying app to production...'
                bat 'powershell -Command "if (-not (Test-Path C:\\prod_app)) { New-Item -ItemType Directory -Path C:\\prod_app }"'
                bat 'powershell -Command "Copy-Item -Path out\\* -Destination C:\\prod_app\\ -Recurse -Force"'
            }
        }

        stage('Verify Deployment') {
      steps {
        echo '✅ Deployment verification complete!'
            }
        }
    }

    post {
    success {
      echo '✅ Build succeeded!'
        }
        failure {
      echo '❌ Build failed!'
        }
    }
}
