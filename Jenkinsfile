pipeline {
    agent any

    environment {
        GITHUB_TOKEN = credentials('github-token')
    }

    stages {

        stage('Build') {
            steps {
                echo 'Building..'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }

    }

    post {
        success {
            script {
                def prStatus = 'success'
                updateGitHubStatus(prStatus)
            }
        }

        failure {
            script {
                def prStatus = 'failure'
                updateGitHubStatus(prStatus)
            }
        }
    }
}

def updateGitHubStatus(String status) {

    withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
        def prUrl = "https://api.github.com/repos/pacots/test-jenkins/statuses/${env.GIT_COMMIT}"
        def data = """
        {
            "state": "${status}",
            "context": "Jenkins CI"
        }
        """
        sh "curl -X POST -H 'Authorization: token ${GITHUB_TOKEN}' -d '${data}' ${prUrl}"
    }
}