pipeline {
    agent any


    stages {
        stage('install ansible prerequisites') {
            steps {
                sh '''
                    ansible-galaxy install jebovic.mailhog
                '''

                sh '''
                    mkdir -p ~/workspace/ansible-project/ansible-test-jenkins/files/certs
                    cd ~/workspace/ansible-project/ansible-test-jenkins/files/certs
                    openssl req -x509 -newkey rsa:4096 -keyout server.key -out server.crt -days 365 --nodes -subj '/C=GR/O=myorganization/OU=it/CN=myorg.com'
                '''
            }
        }
        stage('Prepare mailhog') {            
            steps {
                sshagent (credentials: ['ssh-deploy']) {
                    sh '''
                        pwd
                        echo $WORKSPACE

                        ansible-playbook -i ~/workspace/ansible-project/ansible-test-jenkins/hosts.yml -l deploymentjenkins ~/workspace/ansible-project/ansible-test-jenkins/playbooks/mailhog.yml
                        '''
            }
            
        }
     

        }
    }
}
