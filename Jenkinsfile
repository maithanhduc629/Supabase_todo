pipeline {
    agent any

    environment {
        TELEGRAM_BOT_TOKEN = '8926239435:AAGKiSWAzg-nWlEOm5GDOY2evXPjMOpBinI' // Thay token bot thật của bạn vào
        TELEGRAM_CHAT_ID = '8678496989'       // Thay chat id thật của bạn vào
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
                    def startMsg = "🚀 Bắt đầu deploy website\nRepository: Supabase_todo\nBranch: main\nCommit: ${env.GIT_COMMIT_MSG}"
                    sh "curl -s -X POST https://api.telegram.org/bot${env.TELEGRAM_BOT_TOKEN}/sendMessage -d chat_id=${env.TELEGRAM_CHAT_ID} -d text='${startMsg}'"
                }
            }
        }

        stage('Build & Deploy to Vercel') {
            steps {
                echo 'Đang thực hiện deploy lên Vercel...'
                // Gọi token an toàn từ Jenkins Credentials để deploy thật
                withCredentials([string(credentialsId: 'vercel-token-id', variable: 'VERCEL_TOKEN')]) {
                    sh 'npm install'
                    // Lệnh deploy thực tế lên Vercel (dùng --yes để tự động xác nhận)
                    sh 'npx vercel --token ${VERCEL_TOKEN} --prod --yes'
                }
            }
        }
    }

    post {
        success {
            script {
                def successMsg = "✅ Deploy thành công\nRepository: Supabase_todo\nBranch: main\nWebsite: ${env.VERCEL_PROJECT_URL}"
                sh "curl -s -X POST https://api.telegram.org/bot${env.TELEGRAM_BOT_TOKEN}/sendMessage -d chat_id=${env.TELEGRAM_CHAT_ID} -d text='${successMsg}'"
            }
        }
        failure {
            script {
                // Lấy thông tin lỗi thực tế nếu build/deploy thất bại
                def errorMsg = "❌ Deploy thất bại\nRepository: Supabase_todo\nBranch: main\nCommit: ${env.GIT_COMMIT_MSG}\nError: Quá trình build hoặc deploy lên Vercel gặp lỗi."
                sh "curl -s -X POST https://api.telegram.org/bot${env.TELEGRAM_BOT_TOKEN}/sendMessage -d chat_id=${env.TELEGRAM_CHAT_ID} -d text='${errorMsg}'"
            }
        }
    }
}