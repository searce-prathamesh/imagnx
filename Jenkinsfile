pipeline {
    agent any
    @Library('jenkins-semver')
    
    stages {
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

        stage('Semantic Versioning') {
            steps {
                script {
                    def version = semver.getNextVersion()
                    echo "Generated Semantic Version: ${version}"

                    // You can use the generated version in your artifact naming or publishing
                }
            }
        }

        stage('Publish Artifacts') {
            steps {
                // Publish your Node.js application with the version generated in the previous stage
                // You can use the ${version} variable to name your artifacts accordingly
                echo "${version}"
                // For example, you can create a tarball or ZIP archive of your build output
                sh "tar -czf myapp-${version}.tar.gz -C app ."
            }
        }
    }

    
}
