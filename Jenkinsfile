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
                script {
                    def exists = fileExists '/usr/bin/chef-client'
                    if (exists) {
                        echo "Skipping Chef Workstation install - already installed"
                    } else {
                        sh 'sudo apt-get install -y wget tree unzip'
                        sh 'wget https://packages.chef.io/files/stable/chef-workstation/0.10.41/ubuntu/16.04/chef-workstation_0.10.41-1_amd64.deb'
                        sh 'sudo dpkg -i chef-workstation_0.10.41-1_amd64.deb'
                        sh 'sudo chef env --chef-license accept'
                    }
                }
            }
        }
        stage('Download Apache Cookbook') {
            steps {
                git credentialsId: 'git-repo-creds', url: 'git@github.com:Tyler-Pritchard/apache2.git'
            }
        }
        stage('Install Kitchen Docker Gem') {
            def exists = fileExists '/usr/bin/kitchen-docker'

            steps {
                sh 'chef gem install kitchen-docker'
            }
        }
    }
}