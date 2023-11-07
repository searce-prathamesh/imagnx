pipeline {
    agent any
    tools {
        jfrog 'jfrog-cli'
        nodejs "nodejs"
    }
    stages {
        stage('Build dependancies') {
            steps {
                sh 'npm install'
            }
        }

  //       stage('Version storgae file') {
  //          steps {
  //              script {
  //                  def counter = 0
  //                  def data = "Version" + counter
  //                  writeFile(file: 'version.txt', text: counter.toString())
  //              }
  //          }
  //      }

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

                    def appDirectory = '/var/lib/jenkins/workspace/semantic versioning' // Modify to match your directory structure
                    def packageJsonPath = "${appDirectory}/package.json"

                    if (fileExists(packageJsonPath)) {
                        dir(appDirectory) {
                  //          sh "npm run build -- --env.VERSION=${version}"
                        }
                    } else {
                        error("package.json not found in '${packageJsonPath}'")
                    }
                }
                echo "Build and Package Completed"
            }
        }
       
        
        stage('Publish build info') {
			steps {
				jf 'rt build-publish'
			}
		}
        
    }
}
