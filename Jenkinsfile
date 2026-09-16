pipeline {
    agent any

    stages {
        stage('Validate') {
            steps {
                powershell 'if (-not (Test-Path index.html)) { throw "index.html is missing" }; Select-String -Path index.html -Pattern "Build Bloom" -Quiet'
            }
        }
        stage('Build Docker image') {
            steps {
                powershell 'docker build --tag sample-jenkins:$env:BUILD_NUMBER .'
            }
        }
        stage('Deploy to Nginx') {
            steps {
                powershell '''
                    $target = 'C:\\Users\\pts\\sample-jenkins\\dist\\index.html'
                    New-Item -ItemType Directory -Path (Split-Path -Parent $target) -Force | Out-Null
                    Copy-Item -LiteralPath "$env:WORKSPACE\\index.html" -Destination $target -Force
                '''
            }
        }
    }

    post {
        success { echo 'Build Bloom deployed successfully.' }
        failure { echo 'Build Bloom deployment failed.' }
    }
}
