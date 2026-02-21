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
        stage('Debug Branch Info') {
            steps {
                script {
                    echo "BRANCH_NAME: ${env.BRANCH_NAME}"
                    echo "GIT_BRANCH: ${env.GIT_BRANCH}"
                }
            }
        }
        stage('Restore Dependencies') {
            when {
                expression { 
                    env.BRANCH_NAME == 'main' || 
                    env.GIT_BRANCH == 'main' || 
                    env.GIT_BRANCH == 'origin/main' 
                }
            }
            steps {
                runCmd('dotnet restore')
            }
        }
        stage('Build the app') {
            when {
                expression { 
                    env.BRANCH_NAME == 'main' || 
                    env.GIT_BRANCH == 'main' || 
                    env.GIT_BRANCH == 'origin/main' 
                }
            }
            steps {
                runCmd('dotnet build --no-restore')
            }
        }
        stage('Run Tests') {
            when {
                expression { 
                    env.BRANCH_NAME == 'main' || 
                    env.GIT_BRANCH == 'main' || 
                    env.GIT_BRANCH == 'origin/main' 
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