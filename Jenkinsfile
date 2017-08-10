node {
  // Mark the code checkout 'stage'....
  stage 'Stage Checkout'

  // Checkout code from repository and update any submodules
  checkout scm

  stage 'Stage Build'

  //branch name from Jenkins environment variables
  echo "My branch is:"


  //build your gradle flavor, passes the current build number as a parameter to gradle
  sh "./gradlew assembleDebug"
  sh "./gradlew assembleAndroidTest"

  sh "chmod +x ./scripts/delete-previous-results.sh"
  sh "chmod +x ./scripts/wait-for-file.sh"

  sh "./scripts/delete-previous-results.sh"
    stage 'Run Tests'
 sh "curl -X POST http://192.168.2.135:80/api/v1/execution-plan/execute-test-plan -H 'authorization: Basic a2hhbGVkYTpFeHBlcml0ZXN0MjAxMg==' -H 'cache-control: no-cache' -H 'content-type: multipart/form-data; --form-string 'deviceQueries=@os='\''android'\''' -F testPlan=Default:androidNewSuite --form-string 'app=@/app/build/outputs/apk/app-debug.apk' --form-string 'testApp=@/app/build/outputs/apk/app-debug-androidTest.apk' > ./target/TEST.xml"


  stage 'Stage Archive'
  //tell Jenkins to archive the apks
  archiveArtifacts artifacts: 'app/build/outputs/apk/*.apk', fingerprint: true

//sh "./scripts/wait-for-file.sh"

   stage('Results') {
      junit '**/target/TEST.xml'
      archive 'target/*.jar'
   }


  stage 'Stage Upload To Fabric'
  sh "./gradlew clean"
}

// Pulls the android flavor out of the branch name the branch is prepended with /QA_
