// pipeline {
//     agent any
//     stages {
//         stage('Checkout') {
//             steps {
// //                 checkout([$class: 'GitSCM', branches: [[name: '*/main']], 
// //                 doGenerateSubmoduleConfigurations: false, 
// //                 extensions: [[$class: 'RelativeTargetDirectory', 
// //                 relativeTargetDir: 'project']], submoduleCfg: [], 
// //                 userRemoteConfigs: [[credentialsId: 'git-creds', 
// //                 url: 'https://github.com/vijendrasharma199/SimpleLifecycleApp.git']]])
                
//                 git branch: 'main', credentialsId: '584f87c5-8264-40ce-93dc-6551b1a7fc8b', url: 'git@github.com:vijendrasharma199/SimpleLifecycleApp.git'
//                 //currentBuild.result = 'ABORTED'
//                 error('Stopping early…')
//             }
//         }
//         stage('Build APK') {
//             steps {
//                 sh 'cd project && ./gradlew assembleDebug'
//                 archiveArtifacts artifacts: 'project/app/build/outputs/apk/debug/app-debug.apk', onlyIfSuccessful: true

//             }
//         }
//     }
// //     post {
// //         success {
// //             emailext body: "The Android app build is successful. The APK is attached.", 
// //             subject: "Android app build successful", attachmentsPattern: 'project/app/build/outputs/apk/debug/app-debug.apk', 
// //             to: "vijendra.sharma@sunfox.in"
// //         }
// //         failure {
// //             emailext body: "The Android app build has failed.", 
// //             subject: "Android app build failed", to: "vijendra.sharma@sunfox.in"
// //         }
// //     }
// }

pipeline {
  agent any

  stages {
    stage('Build') {
      steps {
        echo 'Building...'
        sh 'sleep 20'
      }
    }

    stage('Test') {
      steps {
        echo 'Building...'
        sh 'sleep 10'
      }
    }

    stage('Deploy') {
      steps {
        script {
          if (currentBuild.previousBuild.result == 'SUCCESS') {
            echo 'Deploying...'
          } else {
            error('Previous stage failed. Aborting deployment.')
          }
        }
      }
    }
  }
}


