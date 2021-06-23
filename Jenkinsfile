pipeline {
    // 스테이지 별로 다른 거
    agent any

    triggers {
        pollSCM('*/3 * * * *')
    }

    environment {
      AWS_ACCESS_KEY_ID = credentials('awsAccessKeyId')
      AWS_SECRET_ACCESS_KEY = credentials('awsSecretAccessKey')
      AWS_DEFAULT_REGION = 'ap-northeast-2'
      HOME = '.' // Avoid npm root owned
    }

    stages {
        // 레포지토리를 다운로드 받음
        stage('Prepare') {
            agent any
            
            steps {
                echo 'Clonning Repository'

                git url: 'https://github.com/jwlee89-devops/Jenkins.git',
                    branch: 'master',
                    credentialsId: '0ca0afaf-8db8-4dda-bc10-56d8276dfd51'
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    echo 'Successfully Cloned Repository'
                }

                always {
                  echo "i tried..."
                }

                cleanup {
                  echo "after all other post condition"
                }
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
              docker.withRegistry('https://347473060929.dkr.ecr.ap-northeast-2.amazonaws.com/jw-repo-1','er:ap-northeast-2:AWS_Credentials_Jenkins){
                app.push("${env.BUILD_NUMBER}")
                app.push("latest")
            }
          }

          post {
            failure {
              error 'This pipeline stops here...'
            }
          }
        }
    }
}
