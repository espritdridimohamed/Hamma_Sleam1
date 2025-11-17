pipeline {
  agent any
  stages {
    stage('Checkout') { steps { checkout scm } }

    stage('Test') {
      steps {
        sh '''
          mkdir -p test-results
          echo "<testsuite></testsuite>" > test-results/results.xml
          echo "No real tests run" > test-info.txt
        '''
      }
      post { always { junit allowEmptyResults: true, testResults: 'test-results/*.xml' } }
    }

    stage('Build') {
      steps {
        sh '''
          mkdir -p dist
          echo "artifact content" > dist/artifact.txt
          zip -j dist/artifact.zip dist/artifact.txt
        '''
      }
    }

    stage('Deliver') {
      steps {
        archiveArtifacts artifacts: 'dist/**', fingerprint: true
      }
    }
  }
}
