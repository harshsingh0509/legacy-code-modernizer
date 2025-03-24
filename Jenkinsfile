pipeline {
    agent any

    environment {
        FRONTEND_DIR = 'frontend'
        BACKEND_DIR = 'backend'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/harshsingh0509/legacy-code-modernizer.git'
            }
        }

        stage('Backend Setup') {
            steps {
                dir("${BACKEND_DIR}") {
                    echo 'Setting up backend...'
                    sh 'python3 -m pip install -r requirements.txt'
                    sh 'python3 -m unittest discover' // Run tests
                }
            }
        }

        stage('Frontend Setup') {
            steps {
                dir("${FRONTEND_DIR}") {
                    echo 'Setting up frontend...'
                    sh 'npm install'
                    sh 'npm run build'
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                sh '''
                cd backend
                nohup python3 app.pyt & # Start backend
                cd ../frontend
                nohup npm start &        # Start frontend
                '''
            }
        }
    }
}
