pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], 
                doGenerateSubmoduleConfigurations: false, 
                extensions: [[$class: 'RelativeTargetDirectory', 
                relativeTargetDir: 'project']], submoduleCfg: [], 
                userRemoteConfigs: [[credentialsId: 'git-creds', 
                url: 'https://github.com/vijendrasharma199/SimpleLifecycleApp.git']]])
            }
        }
        stage('Build APK') {
            steps {
                sh 'cd project && ./gradlew assembleDebug'
            }
        }
    }
}
