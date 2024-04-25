pipeline {
    agent any

    environment {
        GIT_REPO_URL = 'https://github.com/AntXS/website.git'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], userRemoteConfigs: [[url: GIT_REPO_URL]]])
            }
        }

        stage('Build') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'master') {
                        sh 'docker build -t website-build .'
                    } else if (env.BRANCH_NAME == 'develop') {
                        sh 'docker build -t website-build --build-arg SKIP_PUBLISH=true .'
                    }
                }
            }
        }

        stage('Publish') {
            when {
                branch 'master'
            }
            steps {
                sh 'docker run -d -p 82:80 --name website-container website-build'
            }
        }
    }
}

