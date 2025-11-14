pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/dotnet/sdk:8.0-nanoserver-ltsc2022'
            args '--platform windows'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Verify .NET') {
            steps {
                script {
                    // Проверяем, установлен ли .NET
                    try {
                        sh 'dotnet --version'
                    } catch (Exception e) {
                        error ".NET SDK не установлен на агенте. Установите .NET SDK 6.0 или выше."
                    }
                }
            }
        }
        
        stage('Restore NuGet') {
            steps {
                sh 'dotnet restore'
            }
        }
        
        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release'
            }
        }
        
        stage('Test') {
            steps {
                sh 'dotnet test'
            }
        }
        
        stage('Publish') {
            steps {
                sh 'dotnet publish --configuration Release --output ./publish'
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline завершен'
        }
        success {
            echo 'Успешно!'
            archiveArtifacts artifacts: 'publish/**/*'
        }
        failure {
            echo 'Провал!'
        }
    }
}
