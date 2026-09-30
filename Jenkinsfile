pipeline {
    agent any

    environment {
        TELEGRAM_BOT_TOKEN = '8926239435:AAGKiSWAzg-nWlEOm5GDOY2evXPjMOpBinI' // Thay bằng Bot Token của bạn
        TELEGRAM_CHAT_ID = '8678496989'     // Thay bằng Chat ID của bạn
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Đang lấy mã nguồn từ GitHub...'
                // Thêm checkout scm nếu cần
            }
        }
        stage('Build & Test') {
            steps {
                echo 'Đang chạy build ứng dụng...'
            }
        }
    }

    post {
        success {
            script {
                def message = "✅ Jenkins Build THÀNH CÔNG!\nProject: Supabase_todo\nBranch: main"
                sh "curl -s -X POST https://api.telegram.org/bot${env.TELEGRAM_BOT_TOKEN}/sendMessage -d chat_id=${env.TELEGRAM_CHAT_ID} -d text='${message}'"
            }
        }
        failure {
            script {
                def message = "❌ Jenkins Build THẤT BẠI!\nProject: Supabase_todo\nBranch: main"
                sh "curl -s -X POST https://api.telegram.org/bot${env.TELEGRAM_BOT_TOKEN}/sendMessage -d chat_id=${env.TELEGRAM_CHAT_ID} -d text='${message}'"
            }
        }
    }
}