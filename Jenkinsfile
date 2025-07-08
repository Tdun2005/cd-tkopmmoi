pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                echo 'Cloning source code'
                git branch: 'main', url: 'https://github.com/Tdun2005/cd-tkopmmoi.git'
            }
        }

        stage('Restore Packages') {
            steps {
                echo 'Restoring NuGet packages...'
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project (.NET Core)...'
                bat 'dotnet build --configuration Release'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running unit tests...'
                bat 'dotnet test --no-build --verbosity normal'
            }
        }

        stage('Publish Project') {
            steps {
                echo 'Publishing project to ./publish folder...'
                bat 'dotnet publish -c Release -o ./publish'
            }
        }

        stage('Copy to IIS Folder') {
            steps {
                echo 'Copying publish folder to IIS root...'
                // Dừng IIS để tránh file bị khoá nếu đang chạy
                bat '''
                    iisreset /stop
                    xcopy "%WORKSPACE%\\publish" "C:\\wwwroot\\myproject" /E /Y /I /R
                    iisreset /start
                '''
            }
        }

        stage('Deploy to IIS') {
            steps {
                echo 'Deploying to IIS - creating site if not exists...'
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
