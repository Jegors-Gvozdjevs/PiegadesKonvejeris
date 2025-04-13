pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }
    environment {
        GITHUB_REPO = "https://github.com/mtararujs/python-greetings"
        TEST_REPO = "https://github.com/mtararujs/course-js-api-framework"
    }

    stages {
        stage('install-pip-deps') {
            steps {
                script {
                    echo "Installing all required dependencies..."
                    bat "if exist python-greetings rmdir /s /q python-greetings"
                    bat "git clone %GITHUB_REPO%"
                    dir('python-greetings') {
                        bat "dir"
                        bat "pip3 install -r requirements.txt"
                    }
                }
            }
        }
        stage('Install pm2'){ 
            steps {
                script {
                    echo 'Installing pm2...'
                    bat 'npm install -g pm2'
                    bat 'where npm'
                }
            }
        }

        stage('deploy-to-dev') {
            steps {
                deployApp('dev', '7001')
            }
        }

        stage('tests-on-dev') {
            steps {
                testApp('dev')
            }
        }

        stage('deploy-to-staging') {
            steps {
                deployApp('staging', '7002')
            }
        }

        stage('tests-on-staging') {
            steps {
                testApp('staging')
            }
        }

        stage('deploy-to-preprod') {
            steps {
                deployApp('preprod', '7003')
            }
        }

        stage('tests-on-preprod') {
            steps {
                testApp('preprod')
            }
        }

        stage('deploy-to-prod') {
            steps {
                deployApp('prod', '7004')
            }
        }

        stage('tests-on-prod') {
            steps {
                testApp('prod')
            }
        }
    }
}

def deployApp(envName, port) {
    echo "Deploying to ${envName} environment..."
    bat "if exist python-greetings rmdir /s /q python-greetings"
    bat "git clone %GITHUB_REPO%"
    dir('python-greetings') {
        bat "pm2 delete greetings-app-${envName} & EXIT /B 0"
        bat "pm2 start app.py --name greetings-app-${envName} -- --port ${port}"
    }
}

def testApp(envName) {
    echo "Running tests on ${envName} environment..."
    bat 'IF EXIST course-js-api-framework rmdir /s /q course-js-api-framework'
    bat "git clone %TEST_REPO%"
    dir('course-js-api-framework') {
        bat "npm install"
        bat "npm run greetings greetings_${envName}"
    }
}