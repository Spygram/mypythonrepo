pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
        PYTHON_MAGE = 'spygram/python-image'
        #BACKEND_IMAGE = 'spygram/backend-image'
        IMAGE_TAG = 'latest'
        #BACKEND_TAG = 'latest'
    }
    
    stages {
        stage("SCM") {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Spygram/mypythonrepo.git']])
            }
        }
        
        stage("Build PythonApp") {
            steps {
                script {
                     sh 'docker build -t $PYTHON_IMAGE:$IMAGE_TAG .'
		}
            }
        }
        
#        stage("Build Backend") {
#            steps {
#                script {
#                    dir('backend') {
#                        sh 'docker build -t $BACKEND_IMAGE:$BACKEND_TAG .'
#                    }
#                }
#            }
#        }
        
        stage("Push Python Image") {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials-id') {
                        sh 'docker push $PYTHON_IMAGE:$IMAGE_TAG'
                    }
                }
            }
        }
        
#        stage("Push Backend Image") {
#            steps {
#                script {
#                    docker.withRegistry('', 'dockerhub-credentials-id') {
#                        sh 'docker push $BACKEND_IMAGE:$BACKEND_TAG'
#                    }
#                }
#            }
#        }
#    }
    
    post {
        always {
            cleanWs()
        }
    }
}
