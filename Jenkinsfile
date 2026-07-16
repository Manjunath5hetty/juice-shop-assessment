pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Pull Juice Shop Image') {
            steps {
                sh 'docker pull bkimminich/juice-shop'
            }
        }

        stage('Remove Old Container') {
            steps {
                sh 'docker rm -f juice-shop || true'
            }
        }

        stage('Run Juice Shop') {
            steps {
                sh 'docker run -d --name juice-shop -p 3000:3000 bkimminich/juice-shop'
            }
        }

        stage('Dependency Check') {
            steps {
                dependencyCheck(
                    odcInstallation: 'OWASP',
                    additionalArguments: '--scan . --format XML --format HTML',
                    stopBuild: false
                )
            }
        }

        stage('Publish Report') {
            steps {
                dependencyCheckPublisher(
                    pattern: '**/dependency-check-report.xml'
                )

                archiveArtifacts artifacts: '**/dependency-check-report.html', fingerprint: true
            }
        }
    }
}
