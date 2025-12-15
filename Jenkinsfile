pipeline {
    agent any

    environment {
        VENV_DIR = ".venv"
        HOST = "0.0.0.0"
        PORT = "5000"
        APP_MODULE = "app:app"  
    }



        stage('Setup Virtual Environment') {
            steps {
                bat """
                python -m venv ${VENV_DIR}
                call ${VENV_DIR}\\Scripts\\activate
                python -m pip install --upgrade pip
                pip install -r requirements.txt
                """
            }
        }

        stage('Run Tests') {
            when {
                not {
                    changeset "README.md"
                }
            }
            parallel {
                stage('Test File 1') {
                    steps {
                        bat """
                        call ${VENV_DIR}\\Scripts\\activate
                        python -m pytest test_app.py -v
                        """
                    }
                }
                stage('Test File 2') {
                    steps {
                        bat """
                        call ${VENV_DIR}\\Scripts\\activate
                        python -m pytest test_app_2.py -v
                        """
                    }
                }
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
            emailext(
                to: "EMAIL_TO_PLACEHOLDER",
                from: "EMAIL_FROM_PLACEHOLDER",
                replyTo: "EMAIL_REPLY_PLACEHOLDER",
                subject: "✅ SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                mimeType: 'text/html',
                body: """\
                <p>Good news!</p>
                <p>Build <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b> succeeded.</p>
                <p>Check details: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """
            )
        }

        failure {
            emailext(
                to: "EMAIL_TO_PLACEHOLDER",
                from: "EMAIL_FROM_PLACEHOLDER",
                replyTo: "EMAIL_REPLY_PLACEHOLDER",
                subject: "❌ FAILURE: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
                mimeType: 'text/html',
                body: """\
                <p>Uh oh...</p>
                <p>Build <b>${env.JOB_NAME} #${env.BUILD_NUMBER}</b> failed.</p>
                <p>Check logs: <a href="${env.BUILD_URL}">${env.BUILD_URL}</a></p>
                """
            )
        }
    }
}
