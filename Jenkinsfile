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
    }
	  stage('Docker Build and Push') {
      steps {
        withDockerRegistry([credentialsId: "docker-hub", url: ""]) {
          sh 'printenv'
          sh 'docker build -t asecurityguru/numeric-app:""$GIT_COMMIT"" .'
          sh 'docker push asecurityguru/numeric-app:""$GIT_COMMIT""'
        }
      }
    }
  }
}
