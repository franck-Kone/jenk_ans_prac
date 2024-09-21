pipeline{
    agent {label 'ansible-agent-id'}
       environment {
        VAULT_PASS = credentials('vault-pass-id') // Use Jenkins secret text credential ID
    }

    stages{
        stage('ping nodes'){
            steps{
                sh '''
                echo "$VAULT_PASS" > /tmp/vault_pass.txt
                ansible all -m ping -i /root/inventory.yml --vault-password-file=/tmp/vault_pass.txt
                '''

            }
        }
        stage("create playbooks directory") {
            steps{
                sh "mkdir /root/playbooks"
            }
        }
        stage("move playbooks") {
            steps{
                sh "mv /root/workspace/test1/play* /root/playbooks/play${BUILD_ID}.yml"
            }
        }

        stage("run playbooks") {
            steps{
                sh '''
                ansible-playbook /root/playbooks/play${BUILD_ID}.yml --vault-password-file=/tmp/vault_pass.txt
                
                // Clean up the temporary file
                rm -f /tmp/vault_pass.txt
                '''
            
            }
        }
    }
}