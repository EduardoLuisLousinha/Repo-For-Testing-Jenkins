pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Your build steps here
            }
        }

        stage('Notify Remote') {
            steps {
                script {
                    def remoteMachine = '10.58.0.40'
                    bat "ping -n 4 ${remoteMachine}"
                }
            }
        }
    }
}
