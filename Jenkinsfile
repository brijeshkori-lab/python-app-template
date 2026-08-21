@Library('python-library') _

ipeline {
    agent any

    stages {
        stage('Python CI/CD') {
            steps {
                pythonPipeline('python-local')
            }
        }
    }
}