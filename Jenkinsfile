pipeline { 
  agent any

environment {
    SONAR_SCANNER_VERSION = '5.0.1.3006'
    SONAR_SCANNER_HOME = 'sonar-scanner'
  } 
 
  stages { 
    stage('Checkout') { 
      steps { 
        git branch: 'main', url: ' https://github.com/Kk509/8.2CDevSecOps.git' 
      } 
    } 
 
    stage('Install Dependencies') { 
      steps { 
        sh 'npm install' 
      } 
    } 
 
    stage('Run Tests') { 
      steps { 
        sh 'npm test || true' // Allows pipeline to continue despite test failures 
      } 
    } 
 
    stage('Generate Coverage Report') { 
      steps { 
        // Ensure coverage report exists 
        sh 'npm run coverage || true' 
      } 
    } 
 
    stage('NPM Audit (Security Scan)') { 
      steps { 
        sh 'npm audit || true' // This will show known CVEs in the output 
      } 
    }

    stage('SonarCloud Analysis') {
    steps {
    withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
        sh '''
        # Download SonarScanner CLI
        wget https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux.zip

        # Unzip the scanner
        unzip sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux.zip

        # Rename the folder for easier use
        mv sonar-scanner-${SONAR_SCANNER_VERSION}-linux $SONAR_SCANNER_HOME

        # Add scanner to PATH
        export PATH=$PWD/$SONAR_SCANNER_HOME/bin:$PATH

        # Run SonarScanner
        sonar-scanner -Dsonar.login=$SONAR_TOKEN
        '''
    }
    }
    } 
  } 
} 


