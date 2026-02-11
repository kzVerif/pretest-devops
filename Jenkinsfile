pipeline {
  environment {
    VERCEL_PROJECT_NAME = 'pretest-devops'
    VERCEL_TOKEN = credentials('vercel-token') // ดึงจาก Jenkins
  }
  agent any
      
    tools {
        nodejs 'NodeJS' 
    }

  stages {
    stage('Test npm') {
      steps {
        container('my-builder') {
          sh 'npm --version'
          sh 'node --version'
        }
      }
    }
    stage('Build') {
      steps {
        container('my-builder') {
          sh 'npm ci'
          sh 'npm run build'
        }
      }
    }
    stage('Test Build') {
      steps {
        container('my-builder') {
          sh 'npm run test'
        }
      }
    }
    stage('Deploy') {
      steps {
        container('my-builder') {
          sh 'npm install -g vercel@latest'
          // Deploy using token-only (non-interactive)
          sh '''
            vercel link --project $VERCEL_PROJECT_NAME --token $VERCEL_TOKEN --yes
            vercel --token $VERCEL_TOKEN --prod --confirm
          '''
        }
      }
    }
 
  }
  post {
    always {
      junit 'test-results/junit.xml'
    }
  }
}
