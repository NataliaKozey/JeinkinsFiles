pipeline {
    agent any
    parameters {
        string(name: 'REMOTE_IP', defaultValue: '', description: 'IP Address')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/NataliaKozey/last_version.git',
                        credentialsId: 'github-credentials'
                        ]]
                    ])
            }
        }
        stage('Build') {
            steps {
                script {
                    sh """
                        zip -r latest_crm.zip .
                    """
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'devops-key', keyFileVariable: 'DEVOPS_KEY')]) {
                        sh """
                            ls -l
                            scp -o StrictHostKeyChecking=no  -i \$DEVOPS_KEY latest_crm.zip ubuntu@$REMOTE_IP:/home/ubuntu/
                            ssh -o StrictHostKeyChecking=no -i \$DEVOPS_KEY  ubuntu@$REMOTE_IP 'cd /var/www/html && sudo unzip -o /home/ubuntu/latest_crm.zip && sudo chmod -R 755 . && sudo chown -R www-data:www-data . && sudo chmod -R 775 custom modules themes data upload'

                        """
                    }
                }
            }
        }
    }

}