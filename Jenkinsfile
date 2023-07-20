pipeline {
    agent any
    environment {
        ANSIBLE_SERVER="34.229.178.231"
        JENKINS_PATH="*"
    }
    stages {
        stage('copy files to ansible server') {
            steps {
                script {
                    echo "copying all necessary files to ansible control node"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ${JENKINS_PATH} ubuntu@${ANSIBLE_SERVER}:/home/ubuntu"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                            sh 'scp $keyfile ubuntu@$ANSIBLE_SERVER:/home/ubuntu/ssh-key.pem'
                        }
                    }
                }
            }
        }
        stage("execute ansible playbook") {
            steps{
                script {
                    echo "calling ansible playbook to configure ec2 instances"
                    def remote = [:]
                    remote.name = "ansible-server"
                    remote.host = ANSIBLE_SERVER
                    remote.allowAnyHosts = true

                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                       remote.user = user
                       remote.identityFile = keyfile
                       //sshScript remote: remote, script: "prepare-ansible-server.sh" 
                       sshCommand remote: remote, command: "ls -la"
                    }                    
                }
            }
        }
    }
}
