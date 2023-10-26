pipeline {
    agent any

    stages {
        stage('Update npm') {
            steps {
                sh 'npm install -g npm@latest'
            }
        }
        stage('Checkout') {
            steps {
                // Check out the source code from the repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Use `npm ci` to install project dependencies
                sh 'pnpm install'
            }
        }
    }
}
