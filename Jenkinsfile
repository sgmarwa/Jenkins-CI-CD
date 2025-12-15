pipeline {
    agent any

    environment {
        VENV_DIR = ".venv"
    }

    stages {

        stage('Setup Virtual Environment') {
            steps {
                bat """
                python -m venv %VENV_DIR%
                call %VENV_DIR%\\Scripts\\activate
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Run Tests') {
            steps {
                bat """
                call %VENV_DIR%\\Scripts\\activate
                pytest -v
                """
            }
        }

        stage('Run Application (Local)') {
            steps {
                echo 'Starting Flask app locally using Python server...'
                bat """
                call %VENV_DIR%\\Scripts\\activate
                start /B python app.py
                """
            }
        }
    }

    post {
        success {
            echo "✅ Build succeeded"
        }
        failure {
            echo "❌ Build failed"
        }
    }
}

