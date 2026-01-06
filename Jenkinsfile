pipeline {
    agent any

    environment {
        // 1. Replace with the ID you gave your AWS credentials in Jenkins
        AWS_CRED_ID = 'github-token/******' 
        
        // 2. Replace with your actual S3 bucket name
        S3_BUCKET = 'my-bucket-static189974'
        
        // 3. Replace with your bucket's region (e.g., us-east-1)
        AWS_REGION = 'ap-south-1'
    }

    stages {
        stage('Checkout') {
            steps {
                // This pulls the code from your GitHub repository
                checkout scm
            }
        }

        stage('Deploy to S3') {
            steps {
                // This uses the "Pipeline: AWS Steps" plugin to talk to AWS
                withAWS(credentials: "${AWS_CRED_ID}", region: "${AWS_REGION}") {
                    echo "Uploading files to S3 bucket: ${S3_BUCKET}..."
                    
                    // This command syncs your GitHub files to your S3 bucket
                    // It ignores the .git folder and the Jenkinsfile itself
                    s3Upload(file: '.', bucket: "${S3_BUCKET}", includePathPattern: '**/*', excludePathPattern: '.git/**, Jenkinsfile')
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful! Your website is live.'
        }
        failure {
            echo 'Deployment Failed. Check the Console Output for errors.'
        }
    }
}
