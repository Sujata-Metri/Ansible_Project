pipeline {
    agent any

    environment {
        ANSIBLE_HOST_KEY_CHECKING = 'False'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Sujata-Metri/Ansible_Project.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                wsl sudo apt update
                wsl sudo apt install -y ansible terraform
                '''
            }
        }

        stage('Initialize Terraform') {
            steps {
                dir('terraform') {
                    sh 'wsl terraform init'
                }
            }
        }

        stage('Apply Terraform') {
            steps {
                dir('terraform') {
                    sh 'wsl terraform apply -auto-approve'
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                dir('ansible') {
                    sh 'wsl ansible-playbook -i roles/deploy_static_server/tests/inventory playbook.yaml --become --extra-vars "ansible_become_pass=$BECOME_PASS"'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
        always {
            cleanWs() // Cleanup workspace
        }
    }
}




