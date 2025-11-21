pipeline {

    parameters {
        booleanParam(name: 'autoApprove', defaultValue: false, 
                     description: 'Automatically run apply after generating plan?')
    }

    environment {
        AWS_ACCESS_KEY_ID     = credentials('AWS_ACCESS_KEY_ID')
        AWS_SECRET_ACCESS_KEY = credentials('AWS_SECRET_ACCESS_KEY')
    }

    agent any

    stages {

        stage('Checkout Repo') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/surajdhakad007/terraform-jenkins-AWS-pipelineproject.git'
            }
        }

        stage('Terraform Init & Plan') {
            steps {
                sh '''
                    cd terraform
                    terraform init
                    terraform plan -out=tfplan
                '''
            }
        }

        stage('Approval') {
            when {
                not {
                    equals expected: true, actual: params.autoApprove
                }
            }
            steps {
                script {
                    def planText = sh(
                        script: "cd terraform && terraform show -no-color tfplan",
                        returnStdout: true
                    )

                    input message: "Do you want to apply?",
                          parameters: [
                              text(name: 'Plan', 
                                   description: 'Review Terraform Plan:', 
                                   defaultValue: planText)
                          ]
                }
            }
        }

        stage('Apply') {
            steps {
                sh '''
                    cd terraform
                    terraform apply -input=false tfplan
                '''
            }
        }
    }
}

