
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh 'python3 -m pip install --user -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh 'python3 -m unittest discover -s .'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh '''
                    mkdir -p ${WORKSPACE}/python-app-deploy
                    cp ${WORKSPACE}/app.py ${WORKSPACE}/python-app-deploy/
                '''
            }
        }

        stage('Run Application') {
            steps {
                echo 'Starting application...'
                sh '''
                    cd ${WORKSPACE}/python-app-deploy
                    nohup python3 app.py > app.log 2>&1 &
                    echo $! > app.pid
                '''
            }
        }

        stage('Test Application') {
            steps {
                echo 'Testing application...'
                sh 'python3 ${WORKSPACE}/test_app.py'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs.'
        }
    }
}
