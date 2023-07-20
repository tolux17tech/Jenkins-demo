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
                        withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                        sh 'scp -i  $keyfile ubuntu@${ANSIBLE_SERVER}:/home/ubuntu/ssh-key.pem'
                        }

                    }
                }
            }
        }
        stage("Execute ansible playbook"){
            steps{
                script{
                    echo "Calling ansible playbook to configure EC2 instance"

                    def remote = [:]
                    remote.name = 'ansible-server'
                    remote.host = ANSIBLE_SERVER
                    remote.allowAnyHosts= true
                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                        remote.user = user
                        remote.identityFile = keyfile
                        sshCommand remote: remote, command: "ls -l"
                        }
  
                }
            }
        }
    }
}
