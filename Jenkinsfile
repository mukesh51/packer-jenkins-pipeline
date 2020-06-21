ansiColor('xterm') {
    node {
        stage('Checkout') {
            // Clean the workspace
            cleanWs()
            // Get some code from a GitHub repository
            checkout scm
        }
        stage('Setup') {
            sh "ansible-galaxy install -r requirements.yml"
        }
        stage('Validate') {
            sh "packer validate ubuntu.json"
        }
        stage('Build') {
            withCredentials([usernamePassword(credentialsId: 'aws_access_keys', usernameVariable: 'AWS_ACCESS_KEY', passwordVariable: 'AWS_SECRET_KEY')]) {
            // Run the packer build
            sh "packer build -var 'aws_region=eu-west-2' ubuntu.json"
            }
        }
        stage('Store Artifacts') {
            archiveArtifacts 'manifest.json'
        }
    }
}