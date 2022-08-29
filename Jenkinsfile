pipeline {
  agent any
	tools { 
        maven 'Maven_3_8_5'  
    }

  stages {
    stage('Build Artifact - Maven') {
      steps {
        bat "mvn clean package -DskipTests=true"
        archive 'target/*.jar'
      }
    }

    stage('Unit Tests - JUnit and Jacoco') {
      steps {
        bat "mvn test"
      }
      post {
        always {
          junit 'target/surefire-reports/*.xml'
          jacoco execPattern: 'target/jacoco.exec'
        }
      }
    }
  }
}
