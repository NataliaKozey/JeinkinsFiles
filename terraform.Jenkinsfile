pipeline {
    environment {
        AWS_ACCESS_KEY = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout ([
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/NataliaKozey/terraform.git',
                        credentialsId: 'github-credentials'
                    ]]
                ])
            }

        }

        stage('Terraform Init') {
            steps {
                script {
                    sh '''
                    ls
                    pwd
                    cd terraform-modules
                    terraform init
                    terraform apply --auto-approve | grep "instance_id" | awk '{print $3}' > ip_address.txt
                '''

                }
            }
        }
    }
}