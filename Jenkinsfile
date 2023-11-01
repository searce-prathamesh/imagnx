def majorVersion = 1
def minorVersion = 0
def patchVersion = 0

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
                sh 'yarn install'
                sh "yarn --new-version ${majorVersion}.${minorVersion}.${patchVersion}"
//                script {
 //                   patchVersion = patchVersion + 1
   //             }
                // Example: Package your application and create an artifact with the version
                sh "yarn pack"
                sh "ls -la"
                sh "mv imagix-v${majorVersion}.${minorVersion}.${patchVersion}.tgz my-artifact-${majorVersion}.${minorVersion}.${patchVersion}.tgz"
                archiveArtifacts artifacts: 'my-artifact-*.tgz', allowEmptyArchive: true
            }
        }
    }
}
