pipeline { 
  agent any

  environment {
    SONAR_SCANNER_VERSION = '5.0.1.3006'
    SONAR_SCANNER_HOME = 'sonar-scanner'
  } 

  stages { 
    stage('Checkout') { 
      steps { 
        git branch: 'main', url: 'https://github.com/Kk509/8.2CDevSecOps.git' 
      } 
    } 

    stage('Install Dependencies') { 
      steps { 
        bat 'npm install'
      } 
    } 

    stage('Run Tests') { 
      steps { 
        bat 'npm test || exit 0'  // Allows pipeline to continue despite test failures 
      } 
    } 

    stage('Generate Coverage Report') { 
      steps { 
        bat 'npm run coverage || exit 0' 
      } 
    } 

    stage('NPM Audit (Security Scan)') { 
      steps { 
        bat 'npm audit || exit 0'
      } 
    }

    stage('SonarCloud Analysis') {
  steps {
    withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
      sh '''
        set -e

        # Install wget and unzip if not present
        if ! command -v wget > /dev/null; then
          echo "Installing wget and unzip..."
          apt-get update && apt-get install -y wget unzip
        fi

        # Download the SonarScanner CLI zip
        wget -O sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux.zip

        # Unzip it
        unzip sonar-scanner-cli-${SONAR_SCANNER_VERSION}-linux.zip

        # Rename the folder
        mv sonar-scanner-${SONAR_SCANNER_VERSION}-linux $SONAR_SCANNER_HOME

        # Set SonarScanner home in PATH
        export PATH=$PWD/$SONAR_SCANNER_HOME/bin:$PATH

        # Create a basic sonar-scanner.properties file
        echo "sonar.host.url=https://sonarcloud.io" > $SONAR_SCANNER_HOME/conf/sonar-scanner.properties

        # Run SonarScanner
        sonar-scanner -Dsonar.token=$SONAR_TOKEN
      '''
    }
  }
}

  } 
}
