pipeline {
    agent any
    triggers {
        pollSCM('')
    }
    environment {
        VAULT_SERVER="https://addwebprojects.com:8200"
        ROLE_ID=credentials('jenkins_app_role_id')
        ROLE_SECRET=credentials('jenkins_app_role_secret')
        KV_PATH="devops"
        KV_NAME="addwebprojects.com"
        KV_FIELD="private"
    }
    stages {
        stage('Build') {
            steps {
                script
                    {
                       load "./ansible.groovy"
                    }
                sh './vault_read.sh -u $VAULT_SERVER -r $ROLE_ID -s $ROLE_SECRET -p $KV_PATH -n $KV_NAME -f $KV_FIELD > id_rsa' 
                echo sh(script: 'env|sort', returnStdout: true)
                sh 'echo "${ANSIBLE_HOST} ansible_user=${ANSIBLE_USER} ansible_port=${ANSIBLE_PORT}" > hosts'
                sh 'ansible-playbook main.yml'
            }
        }
    }
}