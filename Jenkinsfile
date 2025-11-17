pipeline {
    agent any
    stages {
        stage('Checkout') { steps { checkout scm } }

        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                        if [ -f mvnw ] || [ -f pom.xml ]; then
                          ./mvnw -B test
                        elif [ -f package.json ]; then
                          npm ci && npm test
                        elif [ -f requirements.txt ] || [ -f pyproject.toml ]; then
                          pip install -r requirements.txt || true
                          pytest -q || true
                        else
                          echo "No known test runner"
                        fi
                        '''
                    } else {
                        bat 'echo Add Windows test commands here'
                    }
                }
            }
            post { always { junit allowEmptyResults: true, testResults: '**/target/surefire-reports/*.xml, **/test-results/*.xml' } }
        }

        stage('Build') {
            steps {
                script {
                    if (isUnix()) {
                        sh '''
                        if [ -f mvnw ] || [ -f pom.xml ]; then
                          ./mvnw -B package -DskipTests
                        elif [ -f package.json ]; then
                          npm run build
                        elif [ -f Dockerfile ]; then
                          docker build -t myapp:latest .
                        else
                          echo "No build step defined"
                        fi
                        '''
                    } else {
                        bat 'echo Add Windows build commands here'
                    }
                }
            }
        }

        stage('Deliver') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar, **/dist/**, **/*.zip', fingerprint: true
            }
        }
    }
}
