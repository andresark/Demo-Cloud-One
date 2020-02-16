pipeline {
  agent any
  stages {
    stage('Git Checkout') {
      parallel {
        stage('Git Checkout') {
          steps {
            git(url: 'https://github.com/felipecosta09/DSSC.git', branch: 'master', poll: true)
          }
        }

        stage('Static Code Analysis') {
          steps {
            sleep 5
          }
        }

      }
    }

    stage('Container Build') {
      steps {
        sh 'docker build -t web-app:latest .'
      }
    }

    stage('Push to ECR') {
      steps {
        sh '''aws --version
$(aws ecr get-login --no-include-email --region us-east-1)
docker tag web-app:latest 650143975734.dkr.ecr.us-east-1.amazonaws.com/web-app
docker push 650143975734.dkr.ecr.us-east-1.amazonaws.com/web-app'''
      }
    }

    stage('Cloud One Container Image Scan') {
      steps {
        echo 'DSSC'
      }
    }

    stage('Dev Tests') {
      parallel {
        stage('Unit Tests') {
          steps {
            sh 'echo \'TBD\''
          }
        }

        stage('Deploy Tests') {
          steps {
            sh 'echo \'TBD\''
          }
        }

        stage('Manual Test') {
          steps {
            input 'Approve?'
          }
        }

      }
    }

    stage('Deploy') {
      parallel {
        stage('Deploy IaaS') {
          steps {
            echo 'Deploy New Container to Fargate'
          }
        }

        stage('Cloud Formation Template Scan') {
          steps {
            sh '''wget https://raw.githubusercontent.com/OzNetNerd/Cloud-Conformity-Pipeline-Scanner/master/code/scanner.py
python3 scanner.py'''
          }
        }

      }
    }

    stage('Slack Notification') {
      steps {
        slackSend(channel: '#aws-account-alerts', color: 'good', message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}", blocks: ' ', attachments: ' ')
      }
    }

  }
}