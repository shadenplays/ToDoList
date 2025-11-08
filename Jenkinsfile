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
          echo "Setting up npm cache..."
          if not exist "${env.NPM_CONFIG_CACHE}" mkdir "${env.NPM_CONFIG_CACHE}"
          npm config set cache "${env.NPM_CONFIG_CACHE}"

          echo "Installing dependencies..."
          npm install
          if %ERRORLEVEL% NEQ 0 (
            echo "npm install failed, trying with --force..."
            npm install --force
          )

          echo "Installing electron-vite..."
          npm install electron-vite --save-dev
        """
      }
    }

    stage('Verify Installation') {
      steps {
        bat """
          echo "Checking installed dependencies..."
          npm list electron-vite
          echo "Current directory:"
          dir
          echo "Package.json contents:"
          type package.json
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

    stage('Prod Server Instrumentation') {
      steps {
        echo "Collecting prod server stats..."

        // CPU usage
        bat 'powershell -Command "Get-CimInstance Win32_Processor | Select-Object LoadPercentage"'

        // Memory usage
        bat 'powershell -Command "Get-CimInstance Win32_OperatingSystem | Select-Object FreePhysicalMemory,TotalVisibleMemorySize"'

        // Disk usage
        bat 'powershell -Command "Get-CimInstance Win32_LogicalDisk | Select-Object DeviceID,FreeSpace,Size"'
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
