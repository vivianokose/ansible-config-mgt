pipeline {
    agent any

    environment {
        REPO_URL = 'https://github.com/vivianokose/ansible-config-mgt.git'
        CREDENTIALS_ID = 'github-token'
        DEPLOY_DIR = '/var/www/html'
        ANSIBLE_INVENTORY = 'inventory.ini'  // Change if your inventory file has a different name
        ANSIBLE_PLAYBOOK = 'site.yml'        // Change if your main playbook has a different name
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

        stage('Run Ansible Playbook') {
            steps {
                echo "Running Ansible playbook..."
                sh '''
                # Install Ansible if not already installed
                if ! command -v ansible-playbook &> /dev/null; then
                    sudo apt update && sudo apt install -y ansible
                fi

                # Run the playbook
                ansible-playbook -i ${ANSIBLE_INVENTORY} ${ANSIBLE_PLAYBOOK} --become
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

