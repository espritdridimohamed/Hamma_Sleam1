pipeline {
  agent any
  stages {
    stage('Checkout') { steps { checkout scm } }

    stage('Test') {
      steps {
        sh '''
          mkdir -p test-results
          cat > test-results/results.xml <<'EOF'
          <?xml version="1.0" encoding="UTF-8"?>
          <testsuite name="dummy" tests="0" failures="0" errors="0" skipped="0"/>
          EOF
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
          tar -czf dist/artifact.tar.gz -C dist artifact.txt
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
