pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'apache_image:v1-master'
        CONTAINER_NAME = 'website-container-master'
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
        stage('Stop and Remove Container') {
            steps {
                script {
                    sh "docker stop ${CONTAINER_NAME} || true"
                    sh "docker rm ${CONTAINER_NAME} || true"
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
        
    }
}
