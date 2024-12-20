pipeline {
    agent any
    environment {
        APACHE_DIR = "/var/www/html"   // Apache web server directory on Azure VM
        VM_IP = "20.118.206.34"       // Your Azure VM public IP
        USERNAME = "azureuser"           // Your Azure VM username
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Deploy to Apache') {
            steps {
                script {
                    // Use Jenkins' SSH credentials to securely copy the file to Azure VM
                    withCredentials([sshUserPrivateKey(credentialsId: 'azure-ssh-key', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                        // SCP command to copy the HTML file to the Apache web directory
                        sh """
                            scp -i \$SSH_PRIVATE_KEY index.html \$USERNAME@\$VM_IP:\$APACHE_DIR
                        """
                    }
                }
            }
        }
    }
    post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
