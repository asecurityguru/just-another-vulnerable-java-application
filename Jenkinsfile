pipeline {
  agent any
  tools { 
        maven 'Maven_3_8_5'  
    }
    stage ('run maven') {
      steps {
            sh 'mvn clean install'     
      }
    }
        
    stage ('archive') {
      steps {
            archiveArtifacts '**/*.jar'
      }
        }
     stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                script{
                 app = docker.build("vulnado")
                }
               }
            }
        }
    stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://145988340565.dkr.ecr.us-west-2.amazonaws.com', 'ecr:us-west-2:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
        }
    stage('Kubernetes Deployment - DEV') {
      steps {
        withKubeConfig([credentialsId: 'kubelogin']) {
          sh "kubectl apply -f deployment.yaml --namespace=test"
        }
      }
    }
  
}
