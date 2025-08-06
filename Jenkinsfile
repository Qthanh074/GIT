pipeline {


    stages {
        stage('Clone') {
            steps {
                echo 'Cloning source code'
                git branch: 'main', url: 'https://github.com/Qthanh074/GIT.git'
            }
        }

        stage('Restore packages') {
            steps {
                echo 'Restoring packages'
                bat 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project'
                bat 'dotnet build --configuration Release'
            }
        }

        stage('Run Tests') {
            steps {
                echo 'Running tests'
                bat 'dotnet test --no-build --verbosity normal'
            }
        }

        stage('Publish') {
            steps {
                echo 'Publishing to ./publish'
                bat 'dotnet publish -c Release -o ./publish'
            }
        }

        stage('Check .NET SDK') {
            steps {
                bat 'dotnet --version'
                bat 'dotnet --list-sdks'
            }
        }

        stage('Copy to IIS') {
            steps {
                echo 'Copying to IIS folder'
                bat 'iisreset /stop'
                bat """
                if exist "%WORKSPACE%\\publish" (
                    xcopy "%WORKSPACE%\\publish" "C:\\inetpub\\wwwroot\\TrienKhai3" /E /Y /I /R
                ) else (
                    echo Publish folder not found!
                    exit /b 1
                )
                """
                bat 'iisreset /start'
            }
        }

        stage('Deploy to IIS') {
            steps {
                echo 'Creating IIS website if not exists'
                powershell '''
                    Import-Module WebAdministration
                    if (-not (Test-Path IIS:\\Sites\\TKP1)) {
                        New-Website -Name "TKP1" -Port 89 -PhysicalPath "C:\\inetpub\\wwwroot\\TrienKhai3" -Force
                    }
                '''
            }
        }
    }
}