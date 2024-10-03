properties([
    parameters([
        string(defaultValue: 'Installation', name: 'Playbook Name'),
        choice(choices: ['Dry-Run','Playbook-deploy'], name: 'Playbook Action')
    ])
])

pipeline {
    agent any 
    stages {
        stage('Preparing') {
            steps {
                sh 'echo Preparing'
            }
        }
        stage('Git Pulling') {
            steps {
                git branch: 'main', url: 'https://github.com/dieunor/cicd-anblible.git'
            }
        }
        stage('Playbook Initializing') {
            steps {
                sh 'echo Playbook Initializing'
            }
        }
        stage('Playbook Running') {
            when {
                expression { params['Playbook Action'] == 'Dry-Run' || params['Playbook Action'] == 'Playbook-deploy' }
            }
            steps {
                script {
                    if (params['Playbook Action'] == 'Dry-Run') {
                        withCredentials([sshUserPrivateKey(credentialsId: 'ansible-connect', keyFileVariable: 'SSH_KEY')]) {
                            sh '''
                            ansible-playbook --check -i /etc/ansible/hosts --private-key "$SSH_KEY" ${params.PlaybookName}.yml
                            '''
                        }
                    } else if (params['Playbook Action'] == 'Playbook-deploy') {
                        ansiblePlaybook credentialsId: 'ansible-connect', disableHostKeyChecking: true, inventory: '/etc/ansible/hosts', playbook: 'nginx-Uninstallation.yml', vaultTmpPath: ''
                    }
                }
            }
        }
        stage('Playbook deployed') {
            steps {
                sh 'echo Deployment done!!!!'
            }
        }
    }
}
