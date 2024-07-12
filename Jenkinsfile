pipeline {
    agent any

    environment {
        SKIP_TESTS = "${params.SKIP_TESTS}"
    }

    parameters {
        booleanParam(name: 'SKIP_TESTS', defaultValue: false, description: 'Skip Tests')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/3nv13m/123.git'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Test') {
            when {
                expression { return !params.SKIP_TESTS }
            }
            steps {
                sh 'coverage run -m unittest discover tests'
                sh 'coverage xml'
            }
            post {
                always {
                    cobertura coberturaReportFile: 'coverage.xml'
                }
            }
        }

        stage('Build') {
            steps {
                sh 'make build'
            }
        }

        stage('Deploy') {
            steps {
                sh 'make deploy'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
        }
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
