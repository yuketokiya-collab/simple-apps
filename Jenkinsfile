pipeline {
    agent { label 'dev01-agus' }
    
    tools { nodejs "nodejs 18.16.0" }
    stages {
        stage('Build') {
            steps {
                sh ''' npm install'''
            }
        }
        stage('Unit Testing') {
            steps {
                sh '''npm test
                '''
            }
        }
        stage('Code Review') {
            steps {
                sh '''sonar-scanner \
                -Dsonar.projectKey=simple-apps \
                -Dsonar.sources=. \
                -Dsonar.host.url=http://172.23.5.3:9000 \
                -Dsonar.login=sqp_0e6db3b92bafc14cef61c3f7f07218b54932a55e'''
            }
        }
        stage('Deploy compose') {
            steps {
                sh '''
                docker compose build
                docker compose up -d
                '''
            }
        }
        stage('Push Images') {
            steps {
                sh '''
                docker tag simple-apps-apps asusant1984/simple-apps-apps
                docker push asusant1984/simple-apps-apps
                docker image prune -a -f
                '''
            }
        }
    }
}