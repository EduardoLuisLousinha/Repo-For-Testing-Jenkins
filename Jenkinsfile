pipeline {
    agent any

    environment {
        // Set any environment variables needed for your build
        GITHUB_USER = 'EduardoLuisLousinha'
        GITHUB_TOKEN = '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'
        GITHUB_REPO = 'EduardoLuisLousinha/Repo-For-Testing-Jenkins'
        CMAKE_HOME = tool 'CMake'
        COMPILER = 'g++' // Change this to your C++ compiler (e.g., 'clang++' or 'g++')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the GitHub repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // Create a build directory
                dir('build') {
                    // Configure and build the C++ project using CMake
                    sh "${CMAKE_HOME}/bin/cmake -DCMAKE_CXX_COMPILER=${COMPILER} .."
                    sh "${CMAKE_HOME}/bin/cmake --build ."
                }
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
        failure {
            // If the build fails, set GitHub commit status to failure
            script {
                updateGitHubCommitStatus('FAILURE', 'Build failed')
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
