pipeline {

    agent any

    stages {

        stage('Deploy to Target') {
            steps {
                sh '''
                    rsync -avz --delete \
                    --exclude='venv' \
                    ./ ubuntu@172.31.43.196:/var/www/python-app/
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    ssh ubuntu@172.31.43.196 "
                    cd /var/www/python-app &&
                    python3 -m venv venv &&
                    ./venv/bin/pip install -r requirements.txt
                    "
                '''
            }
        }

        stage('Restart Application') {
            steps {
                sh '''
                    ssh ubuntu@172.31.43.196 "
                    sudo systemctl restart python-app &&
                    sudo systemctl status python-app --no-pager
                    "
                '''
            }
        }
    }

    post {
        success {
            echo 'Python application deployed successfully!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}
