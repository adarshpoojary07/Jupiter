pipeline {
    agent any

    tools {
        // NodeJS tool name from Jenkins Global Tool Config
        nodejs 'nodejs-lts'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/adarshpoojary07/Jupiter.git'
            }
        }

        stage('Install (optional)') {
            steps {
                // Only if Newman is not installed globally in NodeJS tool
                // bat 'npm install -g newman newman-reporter-htmlextra'
                echo 'Using NodeJS tool (newman should be available if configured in Global Tool).'
            }
        }

        stage('Run Newman') {
            steps {
                // Windows-compatible batch command
                bat """
                    mkdir newman
                    newman run "8.API Chaining with token.postman_collection.json" ^
                        -e "postman_environment.json" ^
                        -r cli,htmlextra,junit ^
                        --reporter-htmlextra-export "newman\\jupi.html" ^
                        --reporter-junit-export "newman\\mars.xml"
                """
            }
        }

        stage('Publish Reports') {
            steps {
                // Archive generated reports
                archiveArtifacts artifacts: 'newman/**', fingerprint: true
                // Optional: publish HTML using HTML Publisher plugin
                // publishHTML([allowMissing: false, alwaysLinkToLastBuild: true, keepAll: true, reportDir: 'newman', reportFiles: 'jupi.html', reportName: 'Newman HTML Report'])
            }
        }
    }

    post {
        always {
            echo "Build finished. Reports archived in newman/."
        }
        failure {
            echo "Newman run failed — check newman/jupi.html and console output."
        }
    }
}
