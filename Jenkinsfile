pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t maxmcf13/myapp:latest -t maxmcf13/myapp:v$BUILD_NUMBER ./flask-app
                '''
                    }
                }
            }
        }
        stage('Push') {
            steps {
                sh '''
                docker push maxmcf13/myapp:latest
                docker push maxmcf13/myapp:v$BUILD_NUMBER
                '''
                    }
            }
        stage('Cleanup') {
            steps {
                sh '''
                docker rmi maxmcf13/myapp:latest
                docker rmi maxmcf13/myapp:v$BUILD_NUMBER
                '''
                  }
            }
        stage('Deploy') {
            steps {
                script {
                    if (env.GIT_BRANCH == 'origin/dev') {
                        sh '''
                        kubectl rollout restart deployment flask-deployment -n pre-prod
                        '''
                    } else if (env.GIT_BRANCH == 'origin/main') {
                        sh '''
                        kubectl rollout restart deployment flask-deployment -n prod
                        '''
                    } else {
                        sh '''
                        echo "Branch not recognised"
                        '''
                    }
                }
            }
        }