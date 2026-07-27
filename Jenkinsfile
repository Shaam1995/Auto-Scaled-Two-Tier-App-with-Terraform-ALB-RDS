pipeline {
    agent any

    environment {
        APP_SERVER = "172.31.2.241"
        APP_USER   = "ubuntu"
        SSH_CRED   = "git-ssh"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Shaam1995/Auto-Scaled-Two-Tier-App-with-Terraform-ALB-RDS.git'
            }
        }

        stage('Deploy HTML') {
            steps {
                sshagent(credentials: [env.SSH_CRED]) {
                    sh '''
                    echo "Workspace Files"
                    ls -la

                    echo "Copying index.html"

                    scp -o StrictHostKeyChecking=no \
                    index.html \
                    ${APP_USER}@${APP_SERVER}:/tmp/

                    ssh -o StrictHostKeyChecking=no \
                    ${APP_USER}@${APP_SERVER} << EOF

                    sudo mv /tmp/index.html /var/www/html/index.html
                    sudo chown www-data:www-data /var/www/html/index.html
                    sudo chmod 644 /var/www/html/index.html
                    sudo systemctl restart apache2

                    EOF

                    echo "Deployment Successful"
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'HTML Deployment Successful'
        }

        failure {
            echo 'HTML Deployment Failed'
        }
    }
}
