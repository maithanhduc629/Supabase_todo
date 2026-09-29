pipeline {
    agent any
    environment {
        VERCEL_TOKEN = credentials('vercel-token-id') 
    }
    stages {
        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }
        stage('Deploy to Vercel') {
            steps {
                sh 'export PATH=$PATH:/usr/bin:/usr/local/bin && npx vercel --token $VERCEL_TOKEN --prod --yes'
            }
        }
    }
    post {
        success {
            echo 'Deploy lên Vercel thành công rực rỡ!'
        }
        failure {
            echo 'Pipeline gặp lỗi, vui lòng kiểm tra lại log.'
        }
    }
}