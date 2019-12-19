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
                git credentialsId: '158f5b63-2d99-4c53-9a39-dc1eec8c8a4c', url: 'git@github.com:Tyler-Pritchard/apache2.git'
            }    
        }
        stage('Install Kitchen Docker Gem') {
            steps {
                sh 'chef gem install kitchen-docker'
            }
        }
        stage("Upload to Chef Infra Server, Converge Nodes") {
            steps {
                withCredentials([zip(credentialsId: 'chef-starter-zip', variable: 'CHEFPRO')]) {
                    sh 'chef install $WORKSPACE/Policyfile.rb -c $CHEFREPO/chef-repo/.chef/knife.rb'
                    sh 'sudo chef push prod $WORKSPACE/Policyfile.lock.json -c $CHEFREPO/chef-repo/.chef/knife.rb'
                    withCredentials([sshUserPrivateKey(credentialsId: 'agent-key', keyFileVariable:
                    'agentKey', passPhraseVariable: ", usernameVariable: ")]) {
                        sh "knife ssh 'policy_name:apache' -x ubuntu -i $agentKey 'sudo chef-client' -c $CHEFREPO/chef-repo/.chef/knife.rb"
                    }
                }
            }
        }
    }
}