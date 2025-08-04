pipeline {
    agent {
        docker { image 'node:14' }
    }

    environment {
        ARTIFACT_NAME = "dist/output.txt"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/balrajpasula/node-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Lint') {
            steps {
                sh 'npm run lint'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: "${ARTIFACT_NAME}", onlyIfSuccessful: true
            }
        }

        stage('Deploy to Staging') {
            steps {
                input message: 'Deploy to staging?', ok: 'Deploy'
                sh 'echo "Deploying to staging server..."'
            }
        }
    }

    post {
        always {
            emailext (
                subject: "Jenkins Build: ${currentBuild.fullDisplayName}",
                body: "Build Result: ${currentBuild.currentResult}",
                recipientProviders: [[$class: 'DevelopersRecipientProvider']],
                to: 'your-email@example.com'
            )
        }
    }
}
