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
                    def responseCode = ''

                    while (responseCode == '200') {
                        responseCode = sh(script: "curl -s -o /dev/null -w '%{http_code}' ${appUrl}", returnStatus: true).toString().trim()
                        echo "Response Code: ${responseCode}"
                        
                        if (responseCode != '200') {
                            echo "ReactJS not Ready!"
                            sleep 3
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
