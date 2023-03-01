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
                archiveArtifacts artifacts: 'project/app/build/outputs/apk/debug/app-debug.apk', onlyIfSuccessful: true

            }
        }
    }
//     post {
//         success {
//             emailext body: "The Android app build is successful. The APK is attached.", 
//             subject: "Android app build successful", attachmentsPattern: 'project/app/build/outputs/apk/debug/app-debug.apk', 
//             to: "vijendra.sharma@sunfox.in"
//         }
//         failure {
//             emailext body: "The Android app build has failed.", 
//             subject: "Android app build failed", to: "vijendra.sharma@sunfox.in"
//         }
//     }
}
