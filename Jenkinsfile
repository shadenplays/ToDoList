pipeline {
  agent any

    environment {
    NPM_CACHE = "${WORKSPACE}\\.npm_cache"
        APP_DIR = "${WORKSPACE}\\my-electron-app"
        PROD_DIR = "C:\\prod_app"
    }

    stages {
    stage('Checkout SCM') {
      steps {
        git branch: 'main', url: 'https://github.com/shadenplays/ToDoList.git'
            }
        }

        stage('Install Dependencies') {
      steps {
        bat """
                    echo Installing dependencies...
                    if not exist "%NPM_CACHE%" mkdir "%NPM_CACHE%"
                    npm config set cache "%NPM_CACHE%"
                    cd "%APP_DIR%"
                    npm install
                """
            }
        }

        stage('Build') {
      steps {
        bat """
                    echo Building Electron app...
                    cd "%APP_DIR%"
                    npx electron-vite build
                """
            }
        }

        stage('Deploy to Prod') {
      steps {
        bat """
                    echo Deploying app to production...
                    if not exist "%PROD_DIR%" mkdir "%PROD_DIR%"
                    robocopy "%APP_DIR%\\dist" "%PROD_DIR%" /MIR
                """
            }
        }

        stage('Verify Deployment') {
      steps {
        bat """
                    echo Verifying deployment...
                    dir "%PROD_DIR%"
                """
                echo 'Deployment verified!'
            }
        }

        stage('Prod Server Instrumentation') {
      steps {
        bat """
                    echo Collecting prod server stats...
                    powershell -Command "Get-CimInstance Win32_Processor | Select-Object LoadPercentage"
                    powershell -Command "Get-CimInstance Win32_OperatingSystem | Select-Object FreePhysicalMemory,TotalVisibleMemorySize"
                    powershell -Command "Get-CimInstance Win32_LogicalDisk | Select-Object DeviceID,FreeSpace,Size"
                """
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
