pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Find Compatible Projects') {
            steps {
                script {
                    sh '''
                    echo "=== Projects compatible with Linux ==="
                    find . -name "*.csproj" ! -exec grep -q "net.*windows" {} \\; -print
                    echo "=== Windows-specific projects (will be skipped) ==="
                    find . -name "*.csproj" -exec grep -l "net.*windows" {} \\; || echo "None"
                    '''
                }
            }
        }
        
        stage('Restore NuGet') {
            steps {
                sh '''
                # Восстанавливаем только проекты без Windows target
                for proj in $(find . -name "*.csproj" ! -exec grep -q "net.*windows" {} \\; -print); do
                    echo "Restoring: $proj"
                    dotnet restore "$proj"
                done
                '''
            }
        }
        
        stage('Build') {
            steps {
                sh '''
                # Собираем только проекты без Windows target
                for proj in $(find . -name "*.csproj" ! -exec grep -q "net.*windows" {} \\; -print); do
                    echo "Building: $proj"
                    dotnet build "$proj" --configuration Release --no-restore
                done
                '''
            }
        }
                
        stage('Publish') {
            steps {
                sh '''
                # Публикуем только проекты без Windows target
                mkdir -p ./publish
                for proj in $(find . -name "*.csproj" ! -exec grep -q "net.*windows" {} \\; -print); do
                    echo "Publishing: $proj"
                    dotnet publish "$proj" --configuration Release --output ./publish --no-build
                done
                '''
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
