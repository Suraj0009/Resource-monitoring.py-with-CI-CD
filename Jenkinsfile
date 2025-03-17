pipeline {
    agent any

    environment {
        VENV_PATH = "/var/www/resource-monitor/venv"
        APP_DIR = "/var/www/resource-monitor"
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    sh "rm -rf ${APP_DIR} && git clone git@github.com:Suraj0009/Resource-monitoring.py-with-CI-CD.git ${APP_DIR}"
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    sh """
                    python3 -m venv ${VENV_PATH}
                    source ${VENV_PATH}/bin/activate
                    pip install -r ${APP_DIR}/requirements.txt
                    """
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh """
                    source ${VENV_PATH}/bin/activate
                    pytest ${APP_DIR}/tests/
                    """
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    sh """
                    sudo systemctl restart resource-monitor
                    """
                }
            }
        }
    }
}
