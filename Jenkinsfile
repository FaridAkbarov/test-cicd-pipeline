pipeline {

  environment {
    registry = "192.168.90.51:5000/farid"
    dockerImage = ""
  }

  my_database_conn = 192.168.10.100:5346
  my_database_password = Password123
  my_database_name = dbname-user
  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/FaridAkbarov/test-cicd-pipeline.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "" ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
