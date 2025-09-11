pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AKIA5P7OOGVBUM43NCVI')   // Jenkins credentials ID
        AWS_SECRET_ACCESS_KEY = credentials('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')
        AWS_DEFAULT_REGION    = 'ap-south-1' // change if needed
        S3_BUCKET             = 'newproject2025'       // replace with your bucket
        CLOUDFRONT_ID         = 'E13Y1I0F4522QA'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Amruta9993/New_project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Frontend') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to S3') {
            steps {
                sh '''
                    aws s3 sync build/ s3://$S3_BUCKET --delete
                '''
            }
        }

        stage('Invalidate CloudFront Cache') {
            steps {
                sh '''
                    aws cloudfront create-invalidation \
                      --distribution-id $CLOUDFRONT_ID \
                      --paths "/*"
                '''
            }
        }
    }

    post {
        failure {
            echo "❌ Build failed. Please check logs."
        }
        success {
            echo "✅ Frontend successfully deployed to S3 and CloudFront cache invalidated."
        }
    }
}
