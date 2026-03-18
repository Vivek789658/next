pipeline{
    agent any

    environment {
        VERCEL_TOKEN = credentials('vercel-token')
    }

    stages {
        stage('Install'){
            steps{
                bat 'npm install'
            }

        }
        stage('Test'){
            steps{
              echo 'skopping tests'
            }

        }
        stage('Build'){
            steps{
                bat 'npm run build'
            }

        }
        stage('Depoly'){
            steps{
                bat 'npx vercel --prod --yes --token=%VERCEL_TOKEN%'
            }

        }
    }
}