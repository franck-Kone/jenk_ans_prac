pipeline{
    agent {label 'ansible-agent-id'}
       environment {
        VAULT_PASS = credentials('vault-pass-id') // Use Jenkins secret text credential ID
    }

    stages{
        stage('ping nodes'){
            steps{
                sh '''
                pwd
                echo "$VAULT_PASS" > /tmp/vault_pass.txt
                ansible all -m ping -i /root/inventory.yml --vault-password-file=/tmp/vault_pass.txt
                '''

                // Clean up the temporary file
                sh '''
                rm -f /tmp/vault_pass.txt
                '''
            }
        }
    }
}