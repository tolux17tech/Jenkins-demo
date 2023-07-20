pipeline {
    agent any
    environment{
        ANSIBLE_SERVER="34.229.178.231"
        JENKINS_PATH="*"
    }
    stages {
        stage("copy files to Jenkins server") {
            steps {
                script {
                    echo "Copy all necessary files to the Ansible control node"
                    sshagent(['ansible-server-key']) {
                        sh 'scp -o StrictHostKeyChecking=no ${JENKINS_PATH} ubuntu@${ANSIBLE_SERVER}:/home/ubuntu'
                        withCredentials([sshUserPrivateKey(credentialsIds: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]){
                         sh "scp $keyfile ubuntu@${ANSIBLE_SERVER}:/home/ubuntu/ssh-key-pem"
                        }
                    }
                }
            }
        }
    }
}
