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
        archiveArtifacts artifacts: '*_version.*'
      }
    }
 }
    post { 
        success {
        sh "git config --global user.name 'raguladora'"
        sh "git config --global user.email 'dorareddy1899@gmail.com'"
        sh "git config --global push.default simple"
        sh "git init"
        sh "git add ."
        sh "git commit -am 'updated'"
        sh "git push -u origin master"
        }
    }
}
