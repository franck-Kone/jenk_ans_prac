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
                mv /root/workspace/test1/Jenkinsfile /root
                echo "$VAULT_PASS" > /tmp/vault_pass.txt
                cd /root
                ansible all -m ping -i inventory.yml --vault-password-file=/tmp/vault_pass.txt
                '''

                // Clean up the temporary file
                sh '''
                rm -f /tmp/vault_pass.txt
                '''
            }
        }
    }
}