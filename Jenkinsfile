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
                bat 'npm install'
            }
        }

        stage('Build') {
            steps {
                bat 'npm run build'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests (none yet)...'
            }
        }

        stage('Archive Build') {
            steps {
                // Change this line to match your actual build output directory
                archiveArtifacts 'out/**/*'
            }
        }
    }

    post {
        failure {
            echo '❌ Build failed!'
        }
        success {
            echo '✅ Build successful!'
        }
    }
}
