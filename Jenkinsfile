pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                git url: 'https://github.com/adarshpoojary07/pipeline.git', branch: 'main'
            }
        }
        stage('Install Newman Reporter') {
            steps {
                bat 'npm install -g newman-reporter-htmlextra'
            }
        }
        stage('Run Postman Tests') {
            steps {
                bat '''
                @echo off
                for /f "tokens=2 delims==" %%I in ('wmic os get localdatetime /value') do set datetime=%%I
                set datetime=%datetime:~0,8%_%datetime:~8,6%
                newman run 8.API_Chaining.json -e postman_environment.json -n 3 --reporters cli,htmlextra --reporter-htmlextra-export "E:/Study_Material/POSTMAN/POSTMAN Collections/newman/Results_%datetime%.html"
                '''
            
            }
       
        }
    }
}