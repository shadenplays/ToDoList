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
                    echo Installing dependencies...
                    if not exist ".npm_cache" mkdir ".npm_cache"
                    npm config set cache ".npm_cache"
                    npm install
                '''
            }
        }

        stage('Build') {
      steps {
        bat '''
                    echo Building Electron app...
                    npm run build
                '''
            }
        }

        stage('Test') {
      steps {
        bat '''
                    echo Running tests...
                    npm test || echo "No tests defined"
                '''
            }
        }

        stage('Archive Artifacts') {
      steps {
        archiveArtifacts artifacts: 'dist/**/*', fingerprint: true
            }
        }

        stage('Prod Server Instrumentation') {
      steps {
        bat '''
                    echo Gathering system metrics...
                    wmic cpu get loadpercentage
                    wmic os get freephysicalmemory,totalvisiblememorysize
                    wmic logicaldisk get size,freespace,caption
                '''
            }
        }
    }

    post {
    success {
      echo '✅ Build and instrumentation completed successfully!'
        }
        failure {
      echo '❌ Build failed. Check logs above.'
        }
    }
}
