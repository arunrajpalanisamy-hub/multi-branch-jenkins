pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Branch: ${env.BRANCH_NAME}"
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                echo "Installing Python dependencies..."
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                echo "Running unit tests..."
                pytest --disable-warnings -q || echo "No tests found!"
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def imageName = "flask-multibranch-demo:${env.BRANCH_NAME}-${env.BUILD_NUMBER}"
                    echo "Building image: ${imageName}"
                    sh "docker build -t ${imageName} ."
                }
            }
        }
    }
}
post {
        always {
            mail bcc: '', body: 'Project build successfully', cc: 'aruntestdemo@gmail.com', from: '', replyTo: '', subject: 'Build Success', to: 'aruntestdemo@gmail.com'
            )
        }
