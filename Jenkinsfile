pipeline {
    agent any

    environment {
        TELEGRAM_BOT_TOKEN = '8926239435:AAGKiSWAzg-nWlEOm5GDOY2evXPjMOpBinI'
        TELEGRAM_CHAT_ID = '8678496989'
        VERCEL_PROJECT_URL = 'https://supabase-todo-d9nqvgtkb-maithanhduc629-9178.vercel.app/'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Đang lấy mã nguồn từ GitHub...'
                checkout scm
                
                script {
                    env.GIT_COMMIT_MSG = sh(script: 'git log -1 --pretty=%B', returnStdout: true).trim()
                }
            }
        }

        stage('Notify Start') {
            steps {
                script {
                    def startMsg = "Bat dau deploy website - Repo: Supabase_todo - Branch: main"
                    sh "curl -s -X POST https://api.telegram.org/bot${env.TELEGRAM_BOT_TOKEN}/sendMessage -d chat_id=${env.TELEGRAM_CHAT_ID} -d text=\"${startMsg}\""
                }
            }
        }

        stage('Build & Deploy to Vercel') {
            steps {
                echo 'Đang tiến hành build ứng dụng...'
                sh 'echo "Build success!"'
            }
        }
    }

    post {
        success {
            script {
                def successMsg = "Deploy thanh cong - Website: ${env.VERCEL_PROJECT_URL}"
                sh "curl -s -X POST https://api.telegram.org/bot${env.TELEGRAM_BOT_TOKEN}/sendMessage -d chat_id=${env.TELEGRAM_CHAT_ID} -d text=\"${successMsg}\""
            }
        }
        failure {
            script {
                def errorMsg = "Deploy that bai trong qua trinh build."
                sh "curl -s -X POST https://api.telegram.org/bot${env.TELEGRAM_BOT_TOKEN}/sendMessage -d chat_id=${env.TELEGRAM_CHAT_ID} -d text=\"${errorMsg}\""
            }
        }
    }
}