pipeline {
    agent any

    environment {
        APP_SERVER = "172.31.11.132"
        APP_USER   = "ubuntu"
        SSH_CRED   = "git-ssh"        // Your Jenkins SSH Credential ID
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Shaam1995/Auto-Scaled-Two-Tier-App-with-Terraform-ALB-RDS.git'
            }
        }

        stage('Deploy Flask App') {
            steps {
                sshagent(credentials: [env.SSH_CRED]) {
                    sh '''
                    echo "Copying Flask application..."

                    scp -r -o StrictHostKeyChecking=no \
                    flask-app \
                    ${APP_USER}@${APP_SERVER}:/home/${APP_USER}/

                    echo "Deploying Flask application..."

                    ssh -o StrictHostKeyChecking=no ${APP_USER}@${APP_SERVER} << EOF

                    cd /home/${APP_USER}/flask-app

                    python3 -m venv venv || true

                    . venv/bin/activate

                    pip install -r requirements.txt

                    pkill -f app.py || true

                    nohup python3 app.py > app.log 2>&1 &

                    exit
EOF
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful'
        }

        failure {
            echo 'Deployment Failed'
        }
    }
}
