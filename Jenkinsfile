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

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Project') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy to Vercel') {
            steps {
                sh 'npx vercel --token $VERCEL_TOKEN --prod --yes'
            }
        }
    }

    post {
        success {
            echo 'Pipeline build và deploy lên Vercel thành công rực rỡ!'
        }
        failure {
            echo 'Pipeline gặp lỗi, vui lòng kiểm tra lại log.'
        }
    }
}
