pipeline { 
  agent any

  environment {
    SONAR_TOKEN = credentials('SONAR_TOKEN')
  } 

  stages { 
    stage('Checkout') { 
      steps { 
        git branch: 'main', url: 'https://github.com/Kk509/8.2CDevSecOps.git' 
      } 
    } 

    stage('Install Dependencies') { 
      steps { 
        sh 'npm install'
      } 
    } 

    stage('Run Tests') { 
      steps { 
        sh 'npm test || exit 0'  // Allows pipeline to continue despite test failures 
      } 
    } 

    stage('Generate Coverage Report') { 
      steps { 
        sh 'npm run coverage || exit 0' 
      } 
    } 

    stage('NPM Audit (Security Scan)') { 
      steps { 
        sh 'npm audit || exit 0'
      } 
    }

stage('SonarCloud Analysis') {
  steps {
            sh '''
        #!/bin/bash
        set -e

        echo "[INFO] Downloading SonarScanner CLI..."
        curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip

        echo "[INFO] Extracting..."
        unzip -q sonar-scanner.zip

        echo "[INFO] Running SonarScanner..."
        export PATH=$PWD/sonar-scanner-5.0.1.3006-linux/bin:$PATH

        sonar-scanner \
            -Dsonar.login=$SONAR_TOKEN
        '''
  }
}
 
  } 
}
