pipeline {
    agent {
        node {
            label 'azure-tools-node01'
        }
      }
    parameters {
        choice(
            name: 'select_env',
            choices: 'prod\ndev',
            description: 'select the environment names'
        )          
        choice(
            name: 'tfvarsfiles',
            choices: 'devops.tfvars\ncore.tfvars',
            description: 'select terraform parameters file to run'
        )

        choice(
            name: 'deploy',
            choices: 'plan\napply',
            description: 'plan or apply terraform configuration to run'
        )
        choice(
            name: 'approvals',
            choices: '\n-auto-approve',
            description: 'approve terraform'
        )
      }
    environment {
        ENV         = "${params.select_env}"       
        DEPLOYMENT  = "${params.deploy}"
        TFVARSFILE  = "${params.tfvarsfiles}"
        APPROVE     = "${params.approvals}"
        WORKDIR_CMD         = '/var/lib/jenkins/devops/'
    }
    stages {
        stage('checkout-repo') {
            steps {
              checkout scm
            }
        }
        stage('init-terraform') {
            steps {
                    sh 'echo $PWD'
		    sh 'cd ${WORKDIR_CMD} && terraform init'
		    sh 'echo $PWD'
            }
        }
        stage('workspace-terraform') {
            steps {
                sh 'cd ${WORKDIR_CMD}'
            }
        }
        stage('deploy-terraform') {
            steps {
                sh 'cd ${WORKDIR_CMD} && terraform $DEPLOYMENT $APPROVE'
            }
        }
    }
}
