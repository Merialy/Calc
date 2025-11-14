pipeline {
    agent {
        docker {
            image 'mcr.microsoft.com/dotnet/sdk:6.0'
            args '-v /var/jenkins_home:/workspace'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Restore NuGet') {
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
                sh 'dotnet test --no-build --verbosity normal'
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
