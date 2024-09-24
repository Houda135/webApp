pipeline {
    agent any

    environment {
        NODE_VERSION = '14.x' // Specify the Node.js version
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Houda135/webApp.git'
            }
        }

        stage('Build') {
            steps {
                script {
                    def nodejs = tool name: 'NodeJS', type: 'NodeJSInstallation'
                    env.PATH = "${nodejs}/bin:${env.PATH}"
                }
                sh 'npm install'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                // This assumes Docker is being used for deployment
                sh 'docker build -t webapp .'
                sh 'docker run -d -p 3000:3000 --name webapp webapp'
            }
        }

        stage('Release') {
            steps {
                // Tag the release in GitHub
                sh 'git tag -a v1.0 -m "Release version 1.0"'
                sh 'git push origin v1.0'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}
