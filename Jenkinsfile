@Library('python-library') _

pipeline {
    agent any

    stages {
        stage('Python CI/CD') {
            steps {
                pythonPipeline('python-local')
            }
        }
    }
}