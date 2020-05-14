pipeline {
 agent any
 stages{
   stage('Create a file with build_number') {
      steps {
        sh "touch {pman_version.cur,pman_version.old}"
        sh "cat pman_version.cur"
        sh "cp pman_version.cur pman_version.old"
        sh "cat pman_version.old"
        sh "echo ${env.BUILD_ID} > pman_version.cur"
        sh "cat pman_version.cur"
      }
    }
 }
}
