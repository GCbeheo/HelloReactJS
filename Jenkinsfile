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
                    def appUrl = 'http://localhost/'
                    def responseCode = sh(script: "curl -s -o /dev/null -w '%{http_code}' ${appUrl}", returnStatus: true).toString().trim()
                    echo "Response Code: ${responseCode}"
                    echo "Type of Response Code: ${responseCode.getClass().getName()}"
                    // def appUrl = 'http://localhost/'
                    // def maxRetries = 5
                    // def retryInterval = 5

                    // for (int i = 0; i < maxRetries; i++) {
                    //     // Kiểm tra sự sẵn sàng của ứng dụng bằng cách gửi yêu cầu HTTP đến endpoint
                    //     def responseCode = sh(script: "curl -s -o /dev/null -w '%{http_code}' ${appUrl}", returnStatus: true).toString().trim()
                    //     print (type(responseCode))
                    //     if (responseCode == 200) {
                    //         echo 'Ứng dụng đã sẵn sàng.'
                    //         break
                    //     } else {
                    //         echo "Đợi ứng dụng sẵn sàng (lần thử lại ${i + 1}/${maxRetries})..."
                    //         sleep retryInterval
                    //     }
                    // }
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
