pipeline {
  agent any

    environment {
    NPM_CACHE = "${WORKSPACE}\\.npm_cache"
        LOCAL_PROJECT = "C:\\Users\\user\\WebstormProjects\\ToDoListApp\\my-electron-app"
        APP_DIR = "${WORKSPACE}\\my-electron-app"
        PROD_DIR = "C:\\prod_app"
    }

    stages {
    stage('Prepare Workspace') {
      steps {
        bat """
                    echo Copying local Electron project to Jenkins workspace...
                    if exist "%APP_DIR%" rmdir /S /Q "%APP_DIR%"
                    xcopy /E /I /Y "%LOCAL_PROJECT%" "%APP_DIR%"
                """
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

        stage('Archive Build') {
      steps {
        archiveArtifacts artifacts: "${APP_DIR}\\dist\\**\\*.*", allowEmptyArchive: false
            }
        }

        stage('Deploy to Prod') {
      steps {
        bat """
                    echo Deploying to production...
                    if not exist "%PROD_DIR%" mkdir "%PROD_DIR%"
                    xcopy /Y /E "%APP_DIR%\\dist" "%PROD_DIR%"
                """
            }
        }
    }

    post {
    success {
      echo "Build and deployment completed successfully!"
        }
        failure {
      echo "Build or deployment failed! Check logs above."
        }
    }
}
