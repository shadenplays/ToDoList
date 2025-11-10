pipeline {
  agent any

    environment {
    // Local npm cache to avoid permission issues
        NPM_CONFIG_CACHE = "${WORKSPACE}\\.npm_cache"
    }

    stages {
    // ------------------------
        stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/shadenplays/ToDoList.git'
            }
        }
      //Clones the repository from your GitHub project’s main branch.
        // ------------------------
        stage('Install Dependencies') {
      steps {
        // Ensure local npm cache exists and use it
                bat "if not exist \"${env.NPM_CONFIG_CACHE}\" mkdir \"${env.NPM_CONFIG_CACHE}\""
                bat "npm config set cache \"${env.NPM_CONFIG_CACHE}\""
                bat "npm install"
                //Checks if the .npm_cache folder exists, and creates it if missing.
                //
                //Configures npm to use that cache folder.
                //
                //Installs all Node.js dependencies listed in package.json.
            }
        }

        // ------------------------
        stage('Build') {
      steps {
        bat "npx electron-vite build"
            }
        }

        // ------------------------
        stage('Test') {
      steps {
        echo "Running tests (none yet)..."
            }
        }

        // ------------------------
        stage('Archive Build') {
      steps {
        // Archive the build output; fail if nothing found
                archiveArtifacts artifacts: 'out/**/*', allowEmptyArchive: false
        //Archives build output files (e.g., from out/) so they are stored in Jenkins and can be downloaded later.
            }
        }

        // ------------------------
        stage('Prod Server Instrumentation') {
      steps {
        echo "Collecting prod server stats..."

        bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -Command "Get-CimInstance Win32_Processor | Select-Object LoadPercentage | Out-String"'
        bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -Command "Get-CimInstance Win32_OperatingSystem | Select-Object FreePhysicalMemory,TotalVisibleMemorySize | Out-String"'
        bat '"C:\\Windows\\System32\\WindowsPowerShell\\v1.0\\powershell.exe" -Command "Get-CimInstance Win32_LogicalDisk | Select-Object DeviceID,FreeSpace,Size | Out-String"'
    }
    //Collects basic system performance and hardware stats from the Jenkins build server using PowerShell commands.
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
