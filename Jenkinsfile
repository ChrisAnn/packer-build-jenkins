ansiColor('xterm') {
    node {
        stage('Checkout') {
            // Get some code from a GitHub repository
            checkout scm
        }
        stage('Setup') {
            sh "ansible-galaxy install -r requirements.yml"
        }
        stage('Validate') {
            sh "packer validate jenkins.json"
            sh "ansible-playbook jenkins.yml --syntax-check"
        }
        stage('Build') {
            withCredentials([usernamePassword(credentialsId: 'aws_access_keys', usernameVariable: 'AWS_ACCESS_KEY', passwordVariable: 'AWS_SECRET_KEY')]) {
            // Run the packer build
                sh "packer build -var 'aws_region=eu-west-1' jenkins.json"
            }
        }
        stage('Store Artifacts') {
            archiveArtifacts 'manifest.json'
        }
    }
}