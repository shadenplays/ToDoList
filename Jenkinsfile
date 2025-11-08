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
                echo 'Running tests...'
                bat 'npm run test -- --reporter=junit --outputFile=test-results/results.xml'
            }
            post {
                always {
                    junit 'test-results/results.xml'
                }
            }
        }

        stage('Archive Build') {
            steps {
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
