pipeline {
    agent {label 'node-1'}

    parameters{
        booleanParam(name: 'DEPLOY', defaultValue: false, description: 'enable to deploy infrastructure')
    }

    stages {
        stage('Deploy') {
            when{
                expression{params.DEPLOY == true}
            }
            steps {
               sh """
                git clone git@github.com:emmanuelcodes2020/ansible-playbook-cma-application.git
                cd ansible-playbook-cma-application
                """

            }
            
          }
          stage('Install Ansible Galaxy Roles') {
            when {
                expression { params.DEPLOY == true }
            }
            steps {
                sh """
                cd ansible-playbook-cma-application
                ansible-galaxy install -r requirements.yml --roles-path roles
                """
            }
        }

        stage('Run Ansible Playbook') {
            when {
                expression { params.DEPLOY == true }
            }
            steps {
                sh """
                cd ansible-playbook-cma-application
                ansible-playbook -i aws_ec2.yml site.yml -b -u ec2-user --private-key=~/.ssh/londonserver.pem                
                """
            }
        }


   }
   post{
        cleanup{
            cleanWs()
        }
   }

        
}
