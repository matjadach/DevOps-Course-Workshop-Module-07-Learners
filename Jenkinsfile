pipeline {
    agent {
        label 'docker'
    }
    stages {
        stage('Build the C# code & test it') {
            agent {
                docker { image 'mcr.microsoft.com/dotnet/sdk:6.0'}
            }
            steps {
                sh 'dotnet build'
                sh 'dotnet test'
            }
        }
        stage('Build the TypeScript code, run the linter & run the tests') {
            agent {
                docker { image 'node:17-bullseye'}
            }
            steps {
                dir('./DotnetTemplate.Web') {
                    sh 'npm install'
                    sh 'npm run build'
                    sh 'npm run lint'
                    sh 'npm t'
                }
            }
        }
    }
}