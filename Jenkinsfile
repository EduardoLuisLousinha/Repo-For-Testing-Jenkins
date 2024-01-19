pipeline {
    agent any

    environment {
        // Set any environment variables needed for your build
        GITHUB_USER = 'EduardoLuisLousinha'
        GITHUB_TOKEN = credentials('208e71ff-7128-47e2-b9c9-3057a464ed99')
        GITHUB_REPO = 'EduardoLuisLousinha/Repo-For-Testing-Jenkins'
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the GitHub repository
                checkout scm
            }
        }
    }

    post {
        success {
            // If the build is successful, set GitHub commit status to success
            script {
                updateGitHubCommitStatus('SUCCESS', 'Build successful')
            }
        }
    }
}

def updateGitHubCommitStatus(String status, String description) {
    script {
        // Get the GitHub commit hash
        def commitHash = sh(script: 'git rev-parse HEAD', returnStdout: true).trim()

        // Update GitHub commit status
        echo "Updating GitHub commit status to ${status}"
        sh """
            curl -u ${env.GITHUB_USER}:${env.GITHUB_TOKEN} \\
            -H "Accept: application/vnd.github.v3+json" \\
            -d '{"state": "${status}", "description": "${description}", "context": "Jenkins"}' \\
            -X POST \\
            "https://api.github.com/repos/${env.GITHUB_REPO}/statuses/${commitHash}"
        """
    }
}
