pipeline {
    agent any

    environment {
        USERNAME ="shaks"
        SERVER_IP = "192.168.56.105"
        DEPLOY_DIR = "/var/www/html"
    }

    stages {
        stage('Build') {
            steps {
                echo "Building project..."
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying workspace files to Apache web server..."
                sh '''
                  rsync -av --delete --exclude ".git/" --exclude "Jenkinsfile" $WORKSPACE/ ${USERNAME}@${SERVER_IP}:${DEPLOY_DIR}/

                  ssh ${USERNAME}@${SERVER_IP} "sudo systemctl restart apache2"
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful! Open http://${SERVER_IP}/ in a browser to see index.html"
        }
        failure {
            echo "❌ Deployment failed!"
        }
    }
}

