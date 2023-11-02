@Library('jenkins-semci')
import ai.stainless.jenkins.ReleaseManager
import ai.stainless.SemverFormatter

def releaseManager = new ReleaseManager(this)
def yqDir = "${env.WORKSPACE}/yq"
releaseManager.prerelease = '%BRANCH_NAME%-%BUILD_NUMBER%'

pipeline {
  agent any
    environment {
        BRANCH_NAME= 'main'
    }   
  stages {
    stage('Versioning') {
      steps {
        script {
          // The version in the app needs to use a "+" to separate the build number
          // but Docker doesn't support that, so we render it using a custom formatter here
          // but use a dash in Dockerhub
          def semver = releaseManager.artifact()
          semver.prerelease = 'main'      //"${env.BRANCH_NAME}"
          echo "prerelease is ${semver.prerelease}"
          semver.buildMetadata = "${currentBuild.number}"
          echo "buildMetadata is ${semver.buildMetadata}"
          versionString = SemverFormatter.ofPattern("M.m.p'-'?P'+'?B").format(semver)
          sh "sudo wget https://github.com/mikefarah/yq/releases/download/3.4.1/yq_linux_amd64 -O /usr/bin/yq"  
          sh "sudo chmod +x /usr/bin/yq"
          sh "pwd"
          sh "sudo yq new '.version=\"${versionString}\"' pubspec.yaml"
          sh "pwd"
        }
      }
    }
    stage('Build') {
      steps {
        // Run your Node.js build command here (e.g., npm run build)
        sh 'npm run build' // Change this to your actual build command
      }
    }
        
    stage('Package') {
      steps {
        // Package your Node.js application here (e.g., creating a distribution)
        sh 'npm pack' // Change this to your actual packaging command
      }
    }
  }
}