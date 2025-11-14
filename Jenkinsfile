pipeline {
    agent any
    
    tools {
        dotnetsdk 'dotnet-sdk-6.0' // если настроено в Global Tools
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Setup .NET') {
            steps {
                script {
                    // Проверка наличия .NET
                    sh 'dotnet --version'
                }
            }
        }
        
        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }
        
        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release --no-restore'
            }
        }
        
        stage('Test') {
            steps {
                sh 'dotnet test --no-build --verbosity normal --logger "trx" --results-directory "TestResults"'
            }
            post {
                always {
                    // Публикация результатов тестов
                    publishHTML([
                        allowMissing: false,
                        alwaysLinkToLastBuild: false,
                        keepAll: true,
                        reportDir: 'TestResults',
                        reportFiles: '*.html',
                        reportName: 'Unit Test Report'
                    ])
                }
            }
        }
        
        stage('Publish') {
            steps {
                sh 'dotnet publish --configuration Release --output ./publish --no-build'
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline завершен'
            cleanWs() // Очистка workspace
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
