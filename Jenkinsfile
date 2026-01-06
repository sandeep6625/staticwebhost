pipeline {
    agent any

    environment {
        AWS_CRED_ID = 'aws-s3-deploy'
        AWS_REGION  = 'ap-south-1'
        S3_BUCKET   = 'my-bucket-static189974'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify AWS Identity') {
            steps {
                withAWS(credentials: AWS_CRED_ID, region: AWS_REGION) {
                    sh 'aws sts get-caller-identity'
                }
            }
        }

        stage('Deploy to S3') {
            steps {
                withAWS(credentials: AWS_CRED_ID, region: AWS_REGION) {
                    sh '''
                       aws s3 sync . s3://${S3_BUCKET} \
                           --exclude ".git/*" \
                           --exclude "Jenkinsfile"
                    '''
                }
            }
        }
    }
}
