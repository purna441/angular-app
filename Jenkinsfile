pipeline {
    agent any

    tools {
        nodejs 'node20'
    }

    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/purna441/angular-app.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build Angular App') {
            steps {
                sh 'npm run build --prod'
            }
        }

        stage('Deploy to S3') {
            steps {
                withAWS(credentials: 'aws-s3-creds', region: 'ap-south-1') {
                    sh '''
                    aws s3 rm s3://bs-jenkins-angular/ --recursive
                    aws s3 cp dist/my-angular-app1/ s3://bs-jenkins-angular/ --recursive
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Completed Successfully!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
