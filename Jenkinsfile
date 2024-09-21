pipeline{
    agent any
       environment {
        VAULT_PASS = credentials('vault-pass-id') // Use Jenkins secret text credential ID
    }

    stages{
        stage('ping nodes'){
            steps{
                sh 'ansible all -m ping --vault-password-file <(echo $VAULT_PASS)'
            }
        }
    }
}