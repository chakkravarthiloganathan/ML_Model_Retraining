
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Detect PKL File') {
            steps {
                script {
                    def files = sh(
                        script: "git diff --name-only HEAD~1 HEAD",
                        returnStdout: true
                    ).trim()

                    if (files.split('\n').any { it.endsWith('.pkl') || it.endsWith('.ppkl') }) {
                        env.RETRAIN = 'true'
                    } else {
                        env.RETRAIN = 'false'
                    }
                }
            }
        }

        stage('Model Retraining') {
            when {
                environment name: 'RETRAIN', value: 'true'
            }
            steps {
                sh 'python3 train.py'
            }
        }
    }
}
