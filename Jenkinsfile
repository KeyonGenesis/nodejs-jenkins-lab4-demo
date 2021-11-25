pipeline {
    agent any
    tools {nodejs "nodejs"}
    
    environment {
        REGISTRY_NAME               = credentials('REGISTRY_NAME')
        DOCKER_REGISTRY_USERNAME    = credentials('DOCKER_REGISTRY_USERNAME')
        DOCKER_REGISTRY_PASSWORD    = credentials('DOCKER_REGISTRY_PASSWORD')
    }
    stages {
        stage('build') {
            steps {
                echo 'Build'
		sh 'npm install'
            }
        }
        stage('test') {
            steps {
                echo 'Test'
            }
        }
        stage('For feature-* branch') {
            when {
                branch 'feature-*'
            }
            steps {
                echo "Feature Branch"
            }
        }
        stage('For MR-* branch') {
            when {
                branch 'MR-*'
            }
            steps {
                echo "Merge Request Branch"
            }
        }
        stage('Deploy Dev') {
            when {
                branch 'dev'
            }
            steps {
                sh '''#!/usr/bin/env bash
                docker login -u ${DOCKER_REGISTRY_USERNAME} -p ${DOCKER_REGISTRY_PASSWORD}
                docker build --tag "${REGISTRY_NAME}/nodejs-jenkins-demo:${BUILD_NUMBER}" .
                docker push "${REGISTRY_NAME}/nodejs-demo-mb:${BUILD_NUMBER}"
                docker run --rm -p 3000:3000 -d --name nodejs-jenkins-lab4-demo "${REGISTRY_NAME}/nodejs-jenkins-demo:${BUILD_NUMBER}"
                echo 'You should now be able to access jenkins at: http://'$(curl -s ifconfig.co)':3000'
                '''
            }
        }
    }
}


