pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/vivianokose/ansible-config-mgt.git'
        CREDENTIALS_ID = 'github-token'
        DEPLOY_DIR = '/var/www/html'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                echo "Cleaning workspace..."
                cleanWs()
            }
        }

        stage('Clone Repository') {
            steps {
                echo "Cloning repo from GitHub..."
                git branch: 'main', url: env.REPO_URL, credentialsId: env.CREDENTIALS_ID
            }
        }

        stage('Build / Test') {
            steps {
                echo "Running build/test stage..."
                sh '''
                # Example commands for testing
                ls -la
                echo "Build/test completed successfully."
                '''
            }
        }

        stage('Deploy to Web Directory') {
            steps {
                echo "Deploying to $DEPLOY_DIR ..."
                sh '''
                sudo rm -rf ${DEPLOY_DIR}/*
                sudo cp -r * ${DEPLOY_DIR}/
                sudo systemctl restart apache2 || sudo systemctl restart httpd || echo "Web server not found"
                echo "Deployment completed."
                '''
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check console output for errors."
        }
    }
}
