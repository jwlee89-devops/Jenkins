pipeline {
  agent any

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
        stage('Test Backend') {
          agent {
            docker {
              image 'node:latest'
            }
          }
          steps {
            echo 'Test Backend'

            dir ('./server'){
                sh '''
                npm install
                npm run test
                '''
            }
          }
        }
        stage('Bulid Backend') {
          agent any
          steps {
            echo 'Build Backend'
            app = docker.build("347473060929.dkr.ecr.ap-northeast-2.amazonaws.com")
            dir ('./server'){
              docker.withRegistry('https://347473060929.dkr.ecr.ap-northeast-2.amazonaws.com/jw-repo-1','er:ap-northeast-2:AWS_Credentials_Jenkins'){
                app.push("${env.BUILD_NUMBER}")
                app.push("latest")
			  }
		    }
		  }
        }
    }
}