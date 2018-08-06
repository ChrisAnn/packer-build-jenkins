pipeline {
    agent any

    options {
        ansiColor('xterm')
    }

    stages {
        stage('Setup') {
            steps {
                sh "ansible-galaxy install -r requirements.yml"
            }
        }
        stage('Validate') {
            steps {
                sh "packer validate jenkins.json"
                sh "ansible-playbook jenkins.yml --syntax-check"
            }
        }
        stage('Build') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws_access_keys', usernameVariable: 'AWS_ACCESS_KEY', passwordVariable: 'AWS_SECRET_KEY')]) {
                    // Run the packer build
                    sh "packer build -var 'aws_region=eu-west-1' jenkins.json"
                }
            }
        }
        stage('Store Artifacts') {
            steps {
                archiveArtifacts 'manifest.json'
            }
        }
    }
}
