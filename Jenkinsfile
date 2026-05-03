pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-east-1'
        S3_BUCKET = 'alberto-final-site'
        CLOUDFRONT_DISTRIBUTION_ID = 'EO8L96LPMM444'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy HTML') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                    aws s3 cp index.html s3://$S3_BUCKET/index.html \
                    --content-type "text/html" \
                    --metadata-directive REPLACE
                    '''
                }
            }
        }

        stage('Deploy CSS') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                    aws s3 cp style.css s3://$S3_BUCKET/style.css \
                    --content-type "text/css" \
                    --metadata-directive REPLACE
                    '''
                }
            }
        }

        stage('Deploy JS') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                    aws s3 cp script.js s3://$S3_BUCKET/script.js \
                    --content-type "application/javascript" \
                    --metadata-directive REPLACE
                    '''
                }
            }
        }

        stage('Invalidate CloudFront') {
            steps {
                withCredentials([
                    string(credentialsId: 'aws-access-key-id', variable: 'AWS_ACCESS_KEY_ID'),
                    string(credentialsId: 'aws-secret-access-key', variable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    sh '''
                    aws cloudfront create-invalidation \
                    --distribution-id $CLOUDFRONT_DISTRIBUTION_ID \
                    --paths "/*"
                    '''
                }
            }
        }
    }
}