pipeline { 
  agent any

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
        sh 'npm test || true'  // Allows pipeline to continue despite test failures 
      } 
    } 

    stage('Generate Coverage Report') { 
      steps { 
        sh 'npm run coverage || true' 
      } 
    } 

    stage('NPM Audit (Security Scan)') { 
      steps { 
        sh 'npm audit || true'
      } 
    }

        stage('SonarCloud Analysis') {
      steps {
        withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
          sh '''
            curl -sSLo sonar-scanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-5.0.1.3006-linux.zip
            unzip sonar-scanner.zip
            export PATH=$PATH:$PWD/sonar-scanner-5.0.1.3006-linux/bin
            sonar-scanner -Dsonar.login=$SONAR_TOKEN
          '''
        }
      }
    }
  } 
}
