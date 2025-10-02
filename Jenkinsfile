// Jenkinsfile (simple declarative) — put at repo root
pipeline {
  agent any

  tools {
    maven 'MAVEN'   // use the exact tool name you set in Manage Jenkins -> Global Tool Configuration
    jdk 'jdk-21'     // same here; adjust as configured
  }

  parameters {
    string(name: 'PORT', defaultValue: '9090', description: 'Port to run app (used only for manual run/deploy)')
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'mvn -B -DskipTests clean package'
      }
    }

    stage('Test') {
      steps {
        sh 'mvn -B test'
      }
    }

    stage('Archive') {
      steps {
        archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
      }
    }
  }

  post {
    always {
      junit '**/target/surefire-reports/*.xml'    // publish test results
      archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
    }
    success {
      echo "Build succeeded: ${env.BUILD_URL}"
    }
    failure {
      echo "Build failed"
    }
  }
}


