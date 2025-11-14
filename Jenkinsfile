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
                bat 'dotnet restore'  // Восстановление зависимостей
            }
        }
        
        stage('Build') {
            steps {
                bat 'dotnet build --configuration Release'  // Сборка
            }
        }
        
        stage('Test') {
            steps {
                bat 'dotnet test'  // Запуск тестов
            }
        }
        
        stage('Publish') {
            steps {
                bat 'dotnet publish --configuration Release --output ./publish'  // Публикация
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline завершен'  // Всегда выполняется
        }
        success {
            echo 'Успешно!'
            archiveArtifacts artifacts: 'publish/**/*'  // Архивируем артефакты
        }
        failure {
            echo 'Провал!'
        }
    }
}
