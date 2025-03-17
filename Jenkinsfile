pipeline {
    agent any

    environment {
        VENV_PATH = "/var/www/resource-monitor/venv"
        APP_DIR = "/var/www/resource-monitor"
    }

    stages {
        stage('Prepare Workspace') {
            steps {
                script {
                    sh """
                    sudo mkdir -p ${APP_DIR}
                    sudo find ${APP_DIR} -mindepth 1 -delete
                    sudo chown -R root:root ${APP_DIR}
                    sudo chmod -R 777 ${APP_DIR}
                    """
                }
            }
        }

        stage('Clone Repository') {
            steps {
                script {
                    sh """
                    sudo rm -rf ${APP_DIR}
                    sudo git clone git@github.com:Suraj0009/Resource-monitoring.py-with-CI-CD.git ${APP_DIR}
                    sudo chown -R root:root ${APP_DIR}
                    sudo chmod -R 777 ${APP_DIR}
                    """
                }
            }
        }

        stage('Setup Virtual Environment & Install Dependencies') {
            steps {
                script {
                    sh """
                    sudo apt update
                    sudo apt install -y python3-venv

                    sudo rm -rf ${VENV_PATH}  # Ensures old venv is deleted
                    python3 -m venv ${VENV_PATH}
                    . ${VENV_PATH}/bin/activate
                    pip install --upgrade pip
                    pip install -r ${APP_DIR}/requirements.txt
                    """
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh """
                    . ${VENV_PATH}/bin/activate
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
