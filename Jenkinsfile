pipeline{
    agent {label 'ansible-agent-label'}
       environment {
        VAULT_PASS = credentials('vault-pass-id') // Use Jenkins secret text credential ID
    }

    stages{
        stage('ping nodes'){
            steps{
                sh '''
                echo "$VAULT_PASS" > /tmp/vault_pass.txt
                ansible all -m ping -i /root/inv.yml --vault-password-file=/tmp/vault_pass.txt

                # Clean up the temporary file
                rm -f /tmp/vault_pass.txt
                '''
            }
        }
        stage("create playbooks directory") {
            steps{
                sh "mkdir /root/playbooks || echo '' "
            }
        }
        // stage("Remove all playbooks") {
        //     steps{
        //         sh "rm -f /root/playbooks/* || echo '' "
        //     }
        // }
        stage("verify folder exists") {
            steps{
                sh "ls /root/playbooks"
            }
        }
        stage("change playbook name in workspace") {
            steps{
                sh "mv /root/workspace/my-pipeline/play* /root/workspace/my-pipeline/playbook_${BUILD_ID}.yml"
            }
        }
        stage("move playbooks") {
            steps{
                sh "mv /root/workspace/my-pipeline/playbook_${BUILD_ID}.yml /root/playbooks/playbook_${BUILD_ID}.yml"
            }
        }
        stage("check playbook") {
            steps{
                sh '''
                echo "$VAULT_PASS" > /tmp/vault_pass.txt
                ansible-playbook -i /root/inv.yml /root/playbooks/playbook_${BUILD_ID}.yml --vault-password-file=/tmp/vault_pass.txt --syntax-check || echo "successfully checked"

                                
                # Clean up the temporary file
                rm -f /tmp/vault_pass.txt
                '''
            }
        }

        stage("run playbooks") {
            steps{
                sh '''
                echo "$VAULT_PASS" > /tmp/vault_pass.txt
                ansible-playbook -i /root/inv.yml /root/playbooks/playbool_${BUILD_ID}.yml --vault-password-file=/tmp/vault_pass.txt
                
                # Clean up the temporary file
                rm -f /tmp/vault_pass.txt
                '''
            }
        }
    }
}