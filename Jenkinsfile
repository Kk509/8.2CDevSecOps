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

  } 
}
