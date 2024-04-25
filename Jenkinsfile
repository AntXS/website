pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'apache_image:v1-develop'
        CONTAINER_NAME = 'website-container-develop'
        DOCKER_PORT_MAPPING = '82:80'
    }
    stages {
        stage('Checkout') {
    steps {
        checkout([$class: 'GitSCM', branches: [[name: 'master'], [name: 'develop']], userRemoteConfigs: [[url: 'https://github.com/AntXS/website.git']]])
    }
}

         
        stage('Pull Code from GitLab') {
            steps {
                          git branch: 'master', url: 'https://github.com/AntXS/website.git'

            }
        
         }
             
        stage('Build') {
            steps {
                script {
                   sh "docker build -t $DOCKER_IMAGE . --no-cache"
                }
            }
        }
        
        stage('Start Container for master') {
            when {
                branch 'master'
            }
            steps {
                script {
                    sh "docker  run -d -p ${DOCKER_PORT_MAPPING} --name ${CONTAINER_NAME} ${DOCKER_IMAGE}"
                }
            }
        }
        stage('No Start for develop') {
            when {
                branch 'develop'
            }
            steps {
                echo "Skipping container start on the develop branch"
            }
        }
    }
}
