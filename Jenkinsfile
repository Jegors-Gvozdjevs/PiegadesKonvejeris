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
                    sh "git clone ${GITHUB_REPO}"
                    dir('python-greetings') {
                        sh "ls -la"
                        sh "pip3 install -r requirements.txt"
                    }
                }
            }
        }

        stage('deploy-to-dev') {
            steps {
                deployApp('dev', 7001)
            }
        }

        stage('tests-on-dev') {
            steps {
                testApp('dev')
            }
        }

        stage('deploy-to-staging') {
            steps {
                deployApp('staging', 7002)
            }
        }

        stage('tests-on-staging') {
            steps {
                testApp('staging')
            }
        }

        stage('deploy-to-preprod') {
            steps {
                deployApp('preprod', 7003)
            }
        }

        stage('tests-on-preprod') {
            steps {
                testApp('preprod')
            }
        }

        stage('deploy-to-prod') {
            steps {
                deployApp('prod', 7004)
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
    sh "git clone ${GITHUB_REPO}"
    dir('python-greetings') {
        sh "pm2 delete greetings-app-${envName} || true"
        sh "pm2 start app.py --name greetings-app-${envName} -- --port ${port}"
    }
}

def testApp(envName) {
    echo "Running tests on ${envName} environment..."
    sh "git clone ${TEST_REPO}"
    dir('course-js-api-framework') {
        sh "npm install"
        sh "npm run greetings greetings_${envName}"
    }
}