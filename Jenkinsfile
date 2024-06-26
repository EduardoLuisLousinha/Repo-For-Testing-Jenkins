pipeline {
    agent any

    environment {
        // Set any environment variables needed for your build
        GITHUB_USER = 'EduardoLuisLousinha'
        GITHUB_TOKEN = credentials('9577eb0f-fa78-4822-a760-9468ddc1f4ef')
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
                updateGitHubCommitStatus('success', 'Build successful')
            }
        }
    }
}

def updateGitHubCommitStatus(String status, String description) {
    script {
        // Get the GitHub commit hash
        def commitHash = bat(script: 'git rev-parse HEAD', returnStdout: true).trim()
        echo "Raw Commit Hash: '${commitHash}'"
        // Split the output by whitespace and take the last element as the commit hash
        def extractHash = commitHash.split(/\s+/).last()
        // Debugging: Print the commit hash
        echo "Extracted Commit Hash: '${extractHash}'"

        // Update GitHub commit status
        echo "Updating GitHub commit status to ${status}"
        bat (script: """
            curl.exe -u ${env.GITHUB_USER}:${env.GITHUB_TOKEN} -H "Accept: application/vnd.github.v3+json" -H "Content-Type: application/json" -d "{\\"state\\": \\"${status}\\", \\"description\\": \\"${description}\\", \\"context\\": \\"Jenkins\\"}" -X POST "https://api.github.com/repos/${env.GITHUB_REPO}/statuses/${extractHash}"
        """, returnStdout:true)
    }
}
