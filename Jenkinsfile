pipeline {
    agent {
        label 'dev-chinhnd-4'
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Bước checkout mã nguồn từ GitHub
                    checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/GCbeheo/HelloReactJS.git']]])
                }
            }
        }
        
        stage('Build and Deploy') {
            steps {
                script {
                    // Bước xây dựng và triển khai bằng Docker Compose
                    sh 'docker-compose -f docker-compose.yaml up -d'
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    def appReady = false
                    def maxRetries = 30
                    def retryInterval = 10

                    for (int i = 0; i < maxRetries; i++) {
                        // Kiểm tra sự sẵn sàng của ứng dụng bằng cách gửi yêu cầu HTTP đến health endpoint
                        def responseCode = sh(script: 'curl -s -o /dev/null -w "%{http_code}" http://localhost/health', returnStatus: true)

                        if (responseCode == 200) {
                            echo 'Ứng dụng đã sẵn sàng.'
                            appReady = true
                            break
                        } else {
                            echo "Đợi ứng dụng sẵn sàng (lần thử lại ${i + 1}/${maxRetries})..."
                            sleep retryInterval
                        }
                    }

                    if (!appReady) {
                        error 'Không thể kết nối đến ứng dụng sau số lần thử lại đã cho.'
                    }
                }
            }
        }

    }
    
//     post {
//         always {
//             // Sau mọi trường hợp, dù là thành công hay thất bại, dừng container
//             script {
//                 sh 'docker-compose -f docker-compose.yaml down'
//             }
//         }
//     }
}
