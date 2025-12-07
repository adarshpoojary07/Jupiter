pipeline {
  agent any

  tools {
    // Name must match the NodeJS tool name you created in Jenkins Global Tool Config
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
        // Only if you didn't add newman as Global npm package in Jenkins tool
        // sh 'npm install -g newman newman-reporter-htmlextra'
        echo 'Using NodeJS tool (newman should be available if configured in Global Tool).'
      }
    }

    stage('Run Newman') {
      steps {
        // Run collection.json; adjust file names as necessary
        // Generate HTML report using htmlextra and write to newman/report.html
        sh '''
          mkdir -p newman
          newman run "8.API Chaining with token.postman_collection.json" \
            -e "postman_environment.json" \
            -r cli,htmlextra,junit \
            --reporter-htmlextra-export "newman/report.html" \
            --reporter-junit-export "newman/report1.xml"
        '''
      }
      // fail build if Newman exit code indicates failures (default behavior)
    }

    stage('Publish Reports') {
      steps {
        // Archive artifacts so you can download the HTML from Jenkins UI
        archiveArtifacts artifacts: 'newman/**', fingerprint: true
        // If you have the HTML Publisher plugin, you can publish the HTML for nicer view
        // publishHTML (not defined here by default) ...
      }
    }
  }

  post {
    always {
      // Print workspace or cleanup if needed
      echo "Build finished. Reports archived in newman/."
    }
    failure {
      echo "Newman run failed — check newman/report.html and console output."
    }
  }
}
