pipeline {

  environment {
      AWS_DEFAULT_REGION = 'ap-northeast-2'
      HOME = '.' // Avoid npm root owned
  }
    stages {
        stage("Git Clone") {
            agent any
            steps{
                echo " First Step "
                echo 'Clonning Repository'

                git url: 'https://github.com/jwlee89-devops/Jenkins.git',
                    branch: 'master',
                    credentialsId: '0ca0afaf-8db8-4dda-bc10-56d8276dfd51'
            }
        }
        stage("Build"){
            echo "Docker File Build Start"
        }
        stage("ECR PUSH"){
            echo "ECR PUSH"
        }
    }

}