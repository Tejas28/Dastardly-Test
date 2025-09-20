// Jenkinsfile (Declarative Pipeline) for integration of Dastardly, from Burp Suite.

pipeline {
    agent any
    stages {
        stage ("Docker Pull Dastardly from Burp Suite container image") {
            steps {
                sh 'docker pull public.ecr.aws/portswigger/dastardly:latest'
            }
        }
        stage ("Docker run Dastardly from Burp Suite Scan") {
            steps {
                cleanWs()
            
                    sh '''
  docker run --user "$(id -u):$(id -g)" --rm \
    -v "${WORKSPACE}:/dastardly" \
    -e "DASTARDLY_TARGET_URL=https://dev-mdashboard.dev.gokwik.in/login#" \
    -e "DASTARDLY_OUTPUT_FILE=/dastardly/dastardly-report.xml" \
    public.ecr.aws/portswigger/dastardly:latest
'''

                
            }
        }
    }
    post {
        always {
            junit testResults: 'dastardly-report.xml', skipPublishingChecks: true
        }
    }
}
