pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Clone Repo') {
            steps {
                git branch: 'main',
                url: 'https://github.com/vivianokose/ansible-config-mgt.git',
                credentialsId: 'github-token'
            }
        }

        stage('Build / Test') {
            steps {
                sh '''
                echo "Running build/test stage..."
                ls -la
                '''
            }
        }

        stage('Deploy to Web Directory') {
            steps {
                sh '''
                echo "Deploying application..."
                sudo rm -rf /var/www/html/*
                sudo cp -r * /var/www/html/
                sudo systemctl restart apache2 || sudo systemctl restart httpd
                '''
            }
        }
    }
}
