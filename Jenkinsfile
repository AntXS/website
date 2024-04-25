pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'apache_image:v1-develop'
        CONTAINER_NAME = 'website-container-develop'
        DOCKER_PORT_MAPPING = '82:80'
    }
    stages {    
        
        stage('Pull Code from GitLab') {
            steps {
                          git branch: 'develop', url: 'https://github.com/AntXS/website.git'

            }
        
         }
             
        stage('Build') {
            steps {
                script {
                   sh "docker build -t $DOCKER_IMAGE . --no-cache"
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
