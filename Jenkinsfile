pipeline {
    agent any

    environment {
        DOTNET_ROOT = 'C:\\Program Files\\dotnet'
        PATH = "${env.PATH};C:\\Program Files\\dotnet"
    }

    stages {
        stage('Clone') {
            steps {
                echo '📥 Cloning source code'
                git branch: 'main', url: 'https://github.com/Tdun2005/cd-tkopmmoi.git'
            }
        }

        stage('Restore Packages') {
            steps {
                echo '🔧 Restoring NuGet packages...'
                bat 'dotnet --version'
                bat 'dotnet restore MyWebApp/MyWebApp.csproj'
            }
        }

        stage('Build') {
            steps {
                echo '🏗️ Building project (.NET Core)...'
                bat 'dotnet build MyWebApp/MyWebApp.csproj --configuration Release'
            }
        }

        stage('Run Tests') {
            steps {
                echo '🧪 Running unit tests...'
                bat 'dotnet test MyWebApp/MyWebApp.csproj --no-build --verbosity normal'
            }
        }

        stage('Publish Project') {
            steps {
                echo '🚀 Publishing project to ./publish folder...'
                bat 'dotnet publish MyWebApp/MyWebApp.csproj -c Release -o ./publish'
            }
        }

        stage('Copy to IIS Folder') {
            steps {
                echo '📁 Copying publish folder to IIS root...'
                bat '''
                    iisreset /stop
                    xcopy "%WORKSPACE%\\publish" "C:\\wwwroot\\myproject" /E /Y /I /R
                    iisreset /start
                '''
            }
        }

        stage('Deploy to IIS') {
            steps {
                echo '🌐 Deploying to IIS...'
                powershell '''
                    Import-Module WebAdministration
                    if (-not (Test-Path IIS:\\Sites\\MySite)) {
                        New-Website -Name "MySite" -Port 81 -PhysicalPath "C:\\test1-netcore" -ApplicationPool ".NET v4.5"
                    }
                '''
            }
        }
    }
}
