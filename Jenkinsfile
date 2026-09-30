pipeline {
    agent any

    environment {
        TELEGRAM_BOT_TOKEN = '8926239435:AAGKiSWAzg-nWlEOm5GDOY2evXPjMOpBinI' // Giữ nguyên Bot Token của bạn
        TELEGRAM_CHAT_ID = '8678496989'     // Giữ nguyên Chat ID của bạn
        VERCEL_TOKEN = 'your_vercel_token'  // Token bảo mật của Vercel (nếu cần dùng CLI)
        VERCEL_PROJECT_URL = 'https://supabase-todo-d9nqvgtkb-maithanhduc629-9178.vercel.app/' // Thay link website Vercel của bạn vào đây
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Đang lấy mã nguồn từ GitHub...'
                checkout scm
                
                // Lấy thông tin Commit Message ngắn gọn để đưa vào thông báo
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
                echo 'Đang thực hiện build và deploy lên Vercel...'
                // Nếu bạn dùng Vercel CLI (hoặc project đã liên kết sẵn Vercel), bạn có thể chạy lệnh deploy:
                // sh "vercel --token ${env.VERCEL_TOKEN} --prod --yes"
                
                // Tạm thời để lệnh giả lập build thành công, bạn thay bằng lệnh deploy thực tế của bạn:
                sh 'npm install'
                sh 'npm run build'
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
                def errorMsg = "❌ Deploy thất bại\nRepository: Supabase_todo\nBranch: main\nCommit: ${env.GIT_COMMIT_MSG}\nError: Lỗi trong quá trình build hoặc deploy pipeline."
                sh "curl -s -X POST https://api.telegram.org/bot${env.TELEGRAM_BOT_TOKEN}/sendMessage -d chat_id=${env.TELEGRAM_CHAT_ID} -d text='${errorMsg}'"
            }
        }
    }
}