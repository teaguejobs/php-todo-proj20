pipeline {
    agent  any
    stages {
        stage('SCM Checkout') {
            steps {
                script {
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/master']],
                        userRemoteConfigs: [[url: 'https://github.com/teaguejobs/php-todo-proj20']]
                    ])
                }
            }
        }
        stage('Build docker image') {
            steps {
                sh 'docker build -t teaguejobs/php-todo:$BUILD_NUMBER .'
            }
        }
        stage('Login to Docker Hub and Push Image') {
            steps{
                withCredentials([usernamePassword(credentialsId: 'docker-hub-teague',
                                                  passwordVariable: 'DOCKERHUB_PSW',
                                                  usernameVariable: 'DOCKERHUB_USR')]) {
                    sh 'echo $DOCKERHUB_PSW | docker login -u $DOCKERHUB_USR --password-stdin'
                    sh 'docker push teaguejobs/php-todo:$BUILD_NUMBER'
                }
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}
