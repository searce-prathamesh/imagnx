pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Make File') {
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
                    def versionCounter = 0

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
                    def version = readFile('version.txt').trim()
                    echo "Using Semantic Version: ${version}"

                    // Set the correct directory for your Node.js application
                    def appDirectory = 'app'  // Modify to match your directory structure
                    def packageJsonPath = "${appDirectory}/package.json"

                    // Check if the package.json file exists
                    if (fileExists(packageJsonPath)) {
                        // Run npm commands within the app directory
                        dir(appDirectory) {
                            nodejs('nodejs') {
                                sh 'npm install'
                                sh "npm run -Dartifactversion=${version}"
                            }
                        }
                    }
                    } 
                    else {
                        error("package.json not found in '${packageJsonPath}'")
                    }
                }
                echo "Build and Package Completed"
            }
        }
    }
}
