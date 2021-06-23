pipeline {

  environment {
      AWS_DEFAULT_REGION = 'ap-northeast-2'
      HOME = '.' // Avoid npm root owned
  }

  stages {
    stage("prepare")
      agent any

      steps {
        echo 'hi welcome to here'
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
}