pipeline {
    agent any

    environment {
        VERCEL_TOKEN = credentials('vercel-token')
    }

    options {
        timeout(time: 30, unit: 'MINUTES') // prevent stuck builds
    }

    stages {

        stage('Clean Workspace') {
            steps {
                echo 'Cleaning old files...'
                bat '''
                rmdir /s /q node_modules 2>nul || echo node_modules not found
                del /f /q package-lock.json 2>nul || echo package-lock.json not found
                '''
            }
        }

        stage('NPM Config Fix') {
            steps {
                echo 'Setting npm configs...'
                bat '''
                npm cache clean --force
                npm config set fetch-retries 5
                npm config set fetch-retry-mintimeout 20000
                npm config set fetch-retry-maxtimeout 120000
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                bat 'npm install --legacy-peer-deps'
            }
        }

        stage('Test') {
            steps {
                echo 'Skipping tests...'
            }
        }

        stage('Build') {
            steps {
                echo 'Building project...'
                bat 'npm run build'
            }
        }

        stage('Deploy to Vercel') {
            steps {
                echo 'Deploying to Vercel...'
                bat 'npx vercel --prod --yes --token=%VERCEL_TOKEN%'
            }
        }
    }

    post {
        success {
            echo '✅ Build & Deployment Successful!'
        }
        failure {
            echo '❌ Pipeline Failed. Check logs.'
        }
        always {
            echo 'Pipeline execution finished.'
        }
    }
}