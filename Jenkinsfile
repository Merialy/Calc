pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm  // Получение кода из Git
            }
        }
        
        stage('Restore NuGet') {
            steps {
                sh 'dotnet restore'  // Восстановление зависимостей
            }
        }
        
        stage('Build') {
            steps {
                sh 'dotnet build --configuration Release'  // Сборка
            }
        }
        
        stage('Test') {
            steps {
                sh 'dotnet test'  // Запуск тестов
            }
        }
        
        stage('Publish') {
            steps {
                sh 'dotnet publish --configuration Release --output ./publish'  // Публикация
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
