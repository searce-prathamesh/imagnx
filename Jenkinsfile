pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }
         stage('Build Node Application') {
            steps {
                script {
                    def counter = 0
                    def data = "Version" + counter
                    writeFile(file: 'version.txt', text: counter.toString())
                }   
        }
        }

        stage('Increment Version') {
            steps {
                script {
                    def versionFile = 'version.txt'

                    // Initialize the version counter
                    def versionCounter = 0

                    // Check if the version file exists
                    if (fileExists(versionFile)) {
                        versionCounter = readFile(versionFile).toInteger() + 1
                    }

                    def version = "Version" + versionCounter

                    echo "Building with version: ${version}"
                    writeFile(file: versionFile, text: versionCounter.toString())
                }
            }
        }

        stage('Build and Package') {
            steps {
                script {
                    // Common code for both 'Increment Version' and 'Build Node Application'
                    def version = readFile('version.txt').trim()
                    echo "Using Semantic Version: ${version}"

                    // Run your Maven package with the version
                    bat "npm run -Dartifactversion=${version}"
                }
                echo "Build and Package Completed"
            }
        }

        stage('Build Node Application') {
            steps {
                // Check out your source code from a version control system (e.g., Git)
                // This assumes your Node.js application is in a directory named 'app'
                checkout scm

                script {
                    // Use Node.js to build your application
                    nodejs('nodejs') {
                        // Install dependencies (e.g., using npm)
                        sh 'npm install'
                    }
                }
            }
        }
    }
}
