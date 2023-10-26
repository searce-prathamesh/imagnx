pipeline {
    agent any

    stages {
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
