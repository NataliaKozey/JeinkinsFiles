pipeline {
    agent any
    parameters {
        string('ip_address', '', 'IP Address')
        string('mysql_root_password', '', 'MySQL Root Password')
        string('myapp_user_password', '', 'MyApp User Password')
    }
    stages {
        stage('Checkout') {
            steps {
                checkout ([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/NataliaKozey/ansible23_lesson.git',
                        credentialsId: 'github-credentials'
                    ]]
                ])
            }

        }

        stage('Run Ansible Playbook') {
            steps {
                sshagent (credentials: ['devops-key']) {
                    sh '''
                    ls -l
                    pwd
                    ansible-playbook server-playbook.yml  --extra-vars ""ip_address=${ip_address}" --extra-vars "mysql_root_password=${mysql_root_password}" --extra-vars "myapp_user_password=${myapp_user_password}" --diff
                    '''

                }
            }
        }
    }
  
}