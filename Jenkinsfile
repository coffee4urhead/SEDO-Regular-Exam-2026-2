def runCmd(String cmd) {
    if (isUnix()) {
        sh cmd
    } else {
        bat cmd
    }
}

pipeline {
    agent any
    stages {
        stage('Restore Dependencies') {
            when {
                anyOf {
                    branch '**/main'
                }
            }
            steps {
                runCmd('dotnet restore')
            }
        }
        stage('Build the app') {
            when {
                anyOf {
                    branch '**/main'
                }
            }
            steps {
                runCmd('dotnet build --no-restore')
            }
        }
        stage('Run Tests') {
            when {
                anyOf {
                    branch '**/main'
                }
            }
            steps {
                runCmd('dotnet test --no-build')
            }
        }
    }

    post {
        success {
            echo "Build succeeded: ${env.BUILD_URL}"
        }
        failure {
            echo "Build failed: ${env.BUILD_URL}"
        }
    }
}