pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from Git repository
                git 'git@github.com:vijendrasharma199/SimpleLifecycleApp.git'
            }
        }
        
        stage('Clean') {
            steps {
                // Clean the project
                sh './gradlew clean'
            }
        }
        
        stage('Build') {
            steps {
                // Build the project
                sh './gradlew assembleDebug'
            }
        }
        
        stage('Save Artifacts') {
            steps {
                // Archive the APK files
                archiveArtifacts artifacts: 'app/build/outputs/**/*.apk', fingerprint: true
            }
        }
    }
}
