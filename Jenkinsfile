node {
   stage('Checkout') {
      // Get some code from a GitHub repository
      git 'https://github.com/ChrisAnn/packer-build-jenkins.git'
   }
    stage('Setup') {
        sh "ansible-galaxy install -r requirements.yml"
    }
   stage('Build') {
       withCredentials([usernamePassword(credentialsId: 'aws_access_keys', usernameVariable: 'AWS_ACCESS_KEY', passwordVariable: 'AWS_SECRET_KEY')]) {
      // Run the packer build
         sh "packer build -var 'aws_region=eu-west-1' jenkins.json"
       }
   }
   stage('Results') {
      sh "cat manifest.json | jq -r '.builds[-1].artifact_id' |  cut -d':' -f2"
      archive 'manifest.json'
   }
}