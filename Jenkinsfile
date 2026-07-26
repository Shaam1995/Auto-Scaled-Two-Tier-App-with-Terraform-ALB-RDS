pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Shaam1995/Auto-Scaled-Two-Tier-App-with-Terraform-ALB-RDS.git'
            }
        }

        stage('Deploy HTML') {
            steps {
                sshagent(['jenkins']) {

                    sh '''
                    echo "Copying HTML file..."

                    scp -o StrictHostKeyChecking=no \
                    index.html \
                    ubuntu@172.31.11.132:/tmp/

                    echo "Deploying..."

                    ssh -o StrictHostKeyChecking=no ubuntu@172.31.11.132 << EOF

                    sudo mv /tmp/index.html /var/www/html/index.html

                    sudo systemctl restart apache2

                    EOF
                    '''
                }
            }
        }

    }
}
