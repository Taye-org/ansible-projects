pipeline {
  agent any

  environment {
    VM_IP = credentials('VM_IP')                //  VM IP address from Jenkins credentials
    VAULT_PASS = credentials('ansible-vault-pass') // Vault password stored as a secret text
  }

  stages {
    stage('Trust VM SSH Host Key') {
      steps {
        sh 'ssh-keyscan -H "$VM_IP" >> ~/.ssh/known_hosts'
      }
    }

    stage('Deploy with Ansible') {
      steps {
        // Save vault password into a file that Ansible can use
        writeFile file: 'vault_pass.txt', text: "${VAULT_PASS}"

        // Run the playbook with vault file
        sh 'ansible-playbook -i "$VM_IP," deploy_Vm.yaml --vault-password-file vault_pass.txt'
      }
    }
  }

  post {
    success {
      echo 'Deployment successful'
    }
    failure {
      echo 'Deployment failed'
    }
  }
}
