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


 sh "./curl -X POST \
  http://192.168.2.135:80/api/v1/execution-plan/execute-test-plan \
  -H 'authorization: Basic a2hhbGVkYTpFeHBlcml0ZXN0MjAxMg==' \
  -H 'cache-control: no-cache' \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -H 'postman-token: 899eb0b7-18ba-cd1a-be22-b77d30ce6021' \
  -F 'app=app/build/outputs/apk/app-debug.apk' \
  -F 'testApp=app/build/outputs/apk/app-debug-androidTest.apk' \
  -F 'deviceQueries=@os='\''android'\''' \
  -F resultType=xml \
  -F testPlan=id:38"


  stage 'Stage Archive'
  //tell Jenkins to archive the apks
  archiveArtifacts artifacts: 'app/build/outputs/apk/*.apk', fingerprint: true


  stage 'Stage Upload To Fabric'
  sh "./gradlew clean"
}

// Pulls the android flavor out of the branch name the branch is prepended with /QA_
