pipeline {
    agent any

    environment {
        APP_SERVER = "ubuntu@172.31.1.176"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Shaam1995/Auto-Scaled-Two-Tier-App-with-Terraform-ALB-RDS.git'
            }
        }

        stage('Deploy') {
            steps {
                sshagent(credentials: ['app-server-ssh']) {
                    sh '''
                        scp -o StrictHostKeyChecking=no index.html ${APP_SERVER}:/tmp/index.html
                        ssh -o StrictHostKeyChecking=no ${APP_SERVER} "sudo mv /tmp/index.html /var/www/html/index.html"
                    '''
                }
            }
        }
    }
}
