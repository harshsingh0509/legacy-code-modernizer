pipeline {
    agent any

    environment {
        FRONTEND_DIR = 'frontend'
        BACKEND_DIR = 'backend'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo 'Cloning repository manually...'
                sh 'git clone https://github.com/harshsingh0509/legacy-code-modernizer.git'
            }
        }

        stage('Backend Setup') {
            steps {
                dir("legacy-code-modernizer/${BACKEND_DIR}") {
                    echo 'Setting up backend...'
                    sh 'python3 -m pip install -r requirements.txt' // Install dependencies
                    sh 'python3 -m unittest discover'               // Run backend tests
                }
            }
        }

        stage('Frontend Setup') {
            steps {
                dir("legacy-code-modernizer/${FRONTEND_DIR}") {
                    echo 'Setting up frontend...'
                    sh 'npm install'            // Install dependencies
                    sh 'npm run build'          // Build production files
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh '''
                cd legacy-code-modernizer/backend
                nohup python3 app.pyt &   // Start backend server
                cd ../frontend
                nohup npm start &         // Start frontend server
                '''
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution completed!'
        }
        failure {
            echo 'Pipeline failed. Check logs for details.'
        }
    }
}
