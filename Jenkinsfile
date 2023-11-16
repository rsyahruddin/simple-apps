pipeline {
    agent {
      label 'server1-rudi'
    }
    
    tools {
      nodejs 'nodejs'
    }
    stages {
        stage('Pull Code') {
            steps {
                git branch: 'main', url: 'https://github.com/rsyahruddin/simple-apps.git'
            }
        }
        
        stage('Build Code') {
            steps {
                sh '''cd apps 
                npm install'''
            }
        }
        
        stage('Testing Code Apps') {
            steps {
                sh '''cd apps
                npm test
                npm run test:coverage'''
            }
        }
        
        stage('Review Code') {
            steps {
                sh '''cd apps
                sonar-scanner \\
                -Dsonar.projectKey=simple-apps-jenkins \\
                -Dsonar.sources=. \\
                -Dsonar.host.url=http://172.23.2.238:9000 \\
                -Dsonar.login=sqp_2e9ca1480a09dc1715e58b99fcccca05604b269f'''
            }
        }
        
        stage('Deploy Code') {
            steps {
                sh '''sed -i \'s/localhost/db/g\' apps/.env
                docker compose build
                docker compose up -d'''
            }
        }
        
    }
}
