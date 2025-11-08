pipeline {
  agent any

    environment {
    NPM_CACHE = "${WORKSPACE}\\.npm_cache"
    }

    stages {
    stage('Checkout SCM') {
      steps {
        git branch: 'main', url: 'https://github.com/shadenplays/ToDoList.git'
            }
        }

        stage('Install Dependencies') {
      steps {
        bat '''
                    echo Installing dependencies...
                    if not exist "%NPM_CACHE%" mkdir "%NPM_CACHE%"
                    npm config set cache "%NPM_CACHE%"
                    cd my-electron-app
                    npm install
                '''
            }
        }

        stage('Build') {
      steps {
        bat '''
                    echo Building Electron app...
                    cd my-electron-app
                    npx electron-vite build
                '''
            }
        }

        stage('Deploy to Prod') {
      steps {
        bat '''
                    echo Deploying app to production...
                    if not exist "C:\\prod_app" mkdir "C:\\prod_app"
                    robocopy "my-electron-app\\dist" "C:\\prod_app" /MIR
                '''
            }
        }

        stage('Verify Deployment') {
      steps {
        bat '''
                    echo Verifying deployment...
                    dir "C:\\prod_app"
                '''
                echo 'Deployment verified!'
            }
        }

        stage('Prod Server Instrumentation') {
      steps {
        bat '''
                    echo Collecting prod server stats...
                    powershell -Command "Get-CimInstance Win32_Processor | Select-Object LoadPercentage"
                    powershell -Command "Get-CimInstance Win32_OperatingSystem | Select-Object FreePhysicalMemory,TotalVisibleMemorySize"
                    powershell -Command "Get-CimInstance Win32_LogicalDisk | Select-Object DeviceID,FreeSpace,Size"
                '''
            }
        }
    }

    post {
    success {
      echo '✔ Build, deploy, and verification successful!'
        }
        failure {
      echo '❌ Build or deployment failed! Check logs above.'
        }
    }
}
