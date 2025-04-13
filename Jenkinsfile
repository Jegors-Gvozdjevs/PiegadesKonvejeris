pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }        
    stages {
        stage('Check npm'){ 
            steps {
                script {
                    echo 'Checking npm version...'
                    bat 'cmd /c chcp 65001 && npm -v'
                }
            }
        }
        stage('Check python'){ 
                steps {
                    script {
                        echo 'Checking npm version...'
                        bat 'python --version'
                    }
                }
            }
        stage('Check pm2'){ 
            steps {
                script {
                    echo 'Installing pm2 globally...'
                    bat 'npm install -g pm2'
                }
            }
        }
    }
}