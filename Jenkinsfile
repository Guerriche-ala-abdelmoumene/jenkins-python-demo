pipeline {
    agent any

    stages {
        // (Build/Install)
        stage('Build & Install') {
            steps {
                echo 'Building...'
               
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
                sh 'pytest --junitxml=test-results.xml'
            }
        }

        stage('Run & Log') {
            steps {
                sh 'python app.py > application.log'
            }
        }
    }

    post {
        always {
            junit 'test-results.xml'
            
            archiveArtifacts artifacts: 'application.log', allowEmptyArchive: true
        }
        success {
            echo 'Great! The build passed successfully.'
        }
        failure {
            echo 'Oops! The build failed.'
        }
    }
}