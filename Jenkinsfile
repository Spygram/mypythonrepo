pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials-id')
        PYTHON_IMAGE = 'spygram/python-image'
        IMAGE_TAG = 'latest'
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
			sh 'docker compose up -d --build'
		}
            }
        }
        
        stage("Push Python Image") {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials-id') {
                        sh 'docker push $PYTHON_IMAGE:$IMAGE_TAG'
                    }
                }
            }
        }
    }    

    post {
        always {
            cleanWs()
        }
    }
}
