pipeline {
    agent any
    tools {
        nodejs "nodejs"
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }

        stage('Increment Version') {
            steps {
                script {
                    def versionFile = 'version.txt'
                    def versionCounter = 0
                    if (fileExists(versionFile)) {
                        versionCounter = readFile(versionFile).toInteger()
                    }
                    versionCounter++
                    def major = env.MAJOR_VERSION ?: 1 // You can set these values as needed
                    def minor = env.MINOR_VERSION ?: 0
                    def patch = versionCounter
                    def version = "${major}.${minor}.${patch}"
                    echo "Incrementing semantic version to: ${version}"
                    writeFile(file: versionFile, text: versionCounter.toString())
                }
            }
        }

        stage('Build and Package') {
            steps {
                script {
                    def version = readFile('version.txt').trim()
                    echo "Using Semantic Version: ${version}"

                    def appDirectory = '/var/lib/jenkins/workspace/semantic-versioning' // Modify to match your directory structure
                    def packageJsonPath = "${appDirectory}/package.json"

                    if (fileExists(packageJsonPath)) {
                        dir(appDirectory) {
                            sh "npm run build -- --env.VERSION=${version}"
                        }
                    } else {
                        error("package.json not found in '${packageJsonPath}'")
                    }
                }
                echo "Build and Package Completed"
            }
        }
    }
}
