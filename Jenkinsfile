pipeline {
    agent any

    stages {
        stage('checkout') {
            steps {
                echo 'chking outt'
                git branch: 'main', credentialsId: 'github_creds', url: 'https://github.com/Siddhartha-chaki/DockerLeanings.git'
            }
        }
        stage('paybook execution') {
            steps {
                echo 'executing playbook'
                ansiblePlaybook credentialsId: 'slv1_auth', disableHostKeyChecking: true, installation: 'ansible1', inventory: 'inventory.ini', playbook: 'playbook.yml'
            }
        }
    }
}