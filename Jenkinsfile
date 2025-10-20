pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = "ap-south-1"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'github-credentials', url: 'https://github.com/Ghani9898/eks-jenkins.git'
            }
        }

        stage('Terraform Init') {
            steps {
                withAWS(credentials: 'aws-credentials', region: "${env.AWS_DEFAULT_REGION}") {
                    sh 'terraform init -input=false'
                }
            }
        }

        stage('Terraform Plan') {
            steps {
                withAWS(credentials: 'aws-credentials', region: "${env.AWS_DEFAULT_REGION}") {
                    sh 'terraform plan -out=tfplan'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                input message: 'Approve deployment?', ok: 'Apply'
                withAWS(credentials: 'aws-credentials', region: "${env.AWS_DEFAULT_REGION}") {
                    sh 'terraform apply -input=false tfplan'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
