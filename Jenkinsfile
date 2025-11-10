pipeline {
    // Run this pipeline only on the Windows slave agent
    agent { label 'win' }

    environment {
        VENV = "${WORKSPACE}\\venv"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Cloning the repository..."
                git url: 'https://github.com/SHEELOJU-SANJAY/Crowd_Emotions.git', branch: 'main'
            }
        }

        stage('Setup Python Environment') {
            steps {
                echo 'Setting up Python virtual environment on Windows...'
                bat '''
                C:\\Users\\sridh\\AppData\\Local\\Microsoft\\WindowsApps\\python.exe -m venv venv
                venv\\Scripts\\python.exe -m pip install --upgrade pip
                
                REM Install dependencies
                IF EXIST requirements.txt (
                    echo "Installing dependencies from requirements.txt"
                    venv\\Scripts\\python.exe -m pip install -r requirements.txt
                ) ELSE (
                    echo "requirements.txt not found, installing pytest manually"
                    venv\\Scripts\\python.exe -m pip install pytest
                )
                '''
            }
        }



        stage('Run Tests') {
            steps {
                echo "Running tests..."
                bat """
                    %VENV%\\Scripts\\pytest.exe tests
                """
            }
        }

        stage('Run Emotion Detection') {
            steps {
                echo "Running Crowd Emotion Detection..."
                bat """
                    %VENV%\\Scripts\\python.exe main.py
                """
            }
        }
    }

    post {
        success {
            echo "Pipeline completed successfully on Windows agent!"
        }
        failure {
            echo "Pipeline failed on Windows agent."
        }
    }
}



