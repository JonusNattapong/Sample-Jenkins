pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate') {
            steps {
                powershell 'if (-not (Test-Path index.html)) { throw "index.html is missing" }; Select-String -Path index.html -Pattern "Build Bloom" -Quiet'
            }
        }
        stage('Build Docker Image') {
            steps {
                powershell 'docker build --tag sample-jenkins:$env:BUILD_NUMBER --tag sample-jenkins:latest .'
            }
        }

        stage('Deploy Container') {
            steps {
                powershell '''
                    docker rm --force sample-jenkins 2>$null
                    docker run --detach --name sample-jenkins --publish 127.0.0.1:8081:80 sample-jenkins:$env:BUILD_NUMBER
                '''
            }
        }

        stage('Verify') {
            steps {
                powershell '''
                    $response = Invoke-WebRequest -Uri 'http://127.0.0.1:8081/' -UseBasicParsing
                    if ($response.StatusCode -ne 200 -or $response.Content -notmatch 'Build Bloom') {
                        throw "Container verification failed"
                    }
                '''
            }
        }
    }

    post {
        always { echo "Pipeline completed for branch $env:BRANCH_NAME." }
        success { echo 'Build Bloom container deployed successfully.' }
        failure { echo 'Build Bloom container deployment failed.' }
    }
}
