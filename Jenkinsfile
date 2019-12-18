pipeline {
    agent { label "agentfarm" }
    stages {
        stage('Delete the workspace') {
            steps{
                sh "sudo rm -rf $WORKSPACE/*"
            }
        }
        stage('Installing Chef Workstation') {
            steps {
                sh 'sudo apt-get install -y wget tree unzip'
                sh 'wget https://packages.chef.io/files/stable/chef-workstation_0.10.41-1_amd.deb'
                sh 'sudo dpkg -i chef-workstation_0.10.41-1.amd64.deb'
                sh 'sudo chef env --chef-lience accept'
            }
        }
        stage('Third Stage') {
            steps {
                echo "Third stage"
            }
        }
    }
}