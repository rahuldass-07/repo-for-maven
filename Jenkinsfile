pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                echo 'Hello from Jenkins Pipeline!'
            }
        }

        stage('Info') {
            steps {
                sh '''
                echo "Build Number: $BUILD_NUMBER"
                echo "Running on: $(hostname)"
                date
                '''
            }
        }
    }
}
