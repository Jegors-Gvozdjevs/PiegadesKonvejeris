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
                    bat 'node -v'
                    bat 'npm -v'
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
    bat """
    echo "Cloning python-greetings ..."
    if exist python-greetings rmdir /s /q python-greetings
    git clone %GITHUB_REPO%
    cd python-greetings
    echo "Stopping existing service if exists..."
    pm2 list | findstr "greetings-app-${envName}" >nul
    if %errorlevel% equ 0 (
        pm2 delete greetings-app-${envName}
    ) else (
        echo "Process greetings-app-${envName} not found, skipping delete."
    )
    echo "Starting service on port ${port}..."
    pm2 start app.py --name greetings-app-${envName} -- --port ${port}
    """
}

def testApp(envName) {
    echo "Running tests on ${envName} environment..."

    // Izdzēst veco mapi, ja tā eksistē
    bat 'IF EXIST course-js-api-framework rmdir /s /q course-js-api-framework'

    // Klonēt testu repozitoriju
    bat 'git clone %TEST_REPO%'

    dir('course-js-api-framework') {
        bat 'npm install'

        // Definēt atbilstošu HOST mainīgo atkarībā no vides
        def port = ""
        if (envName == 'dev')      { port = '7001' }
        else if (envName == 'staging') { port = '7002' }
        else if (envName == 'preprod') { port = '7003' }
        else if (envName == 'prod')    { port = '7004' }

        bat """
        set HOST=http://localhost:${port}
        npm run greetings
        """
    }
}
