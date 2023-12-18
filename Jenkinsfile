pipeline {
    agent {
        label 'dev-chinhnd-4'
    }
    
    stages {
        stage('Checkout') {
            steps {
                script {
                    // Bước checkout mã nguồn từ GitHub
                    git 'https://github.com/GCbeheo/HelloReactJS.git'
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
                    // Bước kiểm thử bằng cách đợi cho đến khi ứng dụng sẵn sàng
                    timeout(time: 60, unit: 'SECONDS') {
                        waitUntil {
                            return sh(script: 'curl -s -o /dev/null -w "%{http_code}" http://localhost/', returnStatus: true) == 200
                        }
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
