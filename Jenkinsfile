pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/adarshpoojary07/Jupiter.git', branch: 'main'
            }
        }

        stage('Install Newman Reporter') {
            steps {
                bat """
                    echo Initializing workspace...
                    npm init -y

                    echo Installing Newman + HTML Reporter locally...
                    npm install newman newman-reporter-htmlextra
                """
            }
        }

        stage('Run Postman Tests') {
            steps {
                bat """
                    @echo off

                    REM --- Generate timestamp ---
                    for /f "tokens=2 delims==" %%I in ('wmic os get localdatetime /value') do set datetime=%%I
                    set datetime=%datetime:~0,8%_%datetime:~8,6%

                    REM --- Run Postman Collection using NPX (important!) ---
                    npx newman run 8.API_Chaining.json -e postman_environment.json -n 3 ^
                    --reporters cli,htmlextra ^
                    --reporter-htmlextra-export "E:/Study_Material/POSTMAN/POSTMAN Collections/newman/Results_%datetime%.html"
                """
            }
        }
    }
}
