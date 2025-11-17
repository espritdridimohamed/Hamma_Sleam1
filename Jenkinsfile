pipeline {
  agent any
  stages {
    stage('Checkout') { steps { checkout scm } }

    stage('Test') {
      steps {
        script {
          if (isUnix()) {
            sh 'if [ -x ./mvnw ]; then ./mvnw -B test; else mvn -B test; fi'
          } else {
            bat 'mvn -B test'
          }
        }
      }
      post { always { junit '**/target/surefire-reports/*.xml' } }
    }

    stage('Build') {
      steps {
        script {
          if (isUnix()) {
            sh 'if [ -x ./mvnw ]; then ./mvnw -B package -DskipTests; else mvn -B package -DskipTests; fi'
          } else {
            bat 'mvn -B package -DskipTests'
          }
        }
      }
    }

    stage('Run') {
      steps {
        script {
          if (isUnix()) {
            // run the app using the classpath (works if Main-Class is not in manifest)
            sh 'ls -la target || true'
            sh 'java -cp target/demo-1.0-SNAPSHOT.jar com.example.App'
          } else {
            bat 'dir target'
            bat 'java -cp target\\demo-1.0-SNAPSHOT.jar com.example.App'
          }
        }
      }
    }

    stage('Deliver') {
      steps {
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
      }
    }
  }
  post {
    success { echo 'Pipeline success' }
    failure { echo 'Pipeline failed' }
  }
}
