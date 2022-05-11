pipeline {
    agent none
    stages {
        stage('Build the C# code & test it') {
            agent {
                docker { image 'mcr.microsoft.com/dotnet/sdk:6.0'}
            }
            environment {
                DOTNET_CLI_HOME = "/tmp/dotnet_cli_home"
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
                    sh 'npm run test-with-coverage'
                    sh 'publishCoverage adapters: [istanbulCoberturaAdapter('coverage/cobertura-coverage.xml')]'
                }
            }
        }
    }
}