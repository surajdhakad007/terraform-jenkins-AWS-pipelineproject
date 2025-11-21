
pipeline {
    agent any

    environment {
        AWS_ACCESS_KEY_ID = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    stages {

        stage('Checkout Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/surajdhakad007/terraform-jenkins-AWS-pipelineproject.git'
            }
        }

        stage('Terraform Init & Plan') {
            steps {
                sh '''
                terraform init
                terraform plan -out=tfplan
                '''
            }
        }

        stage('Approval') {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    input message: 'Do you want to apply changes?'
                }
            }
        }

        stage('Apply') {
            steps {
                sh '''
                terraform apply -auto-approve tfplan
                '''
            }
        }
    }
}
