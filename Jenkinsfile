pipeline {
 agent any
 stages{
//   stage('Create a file with build_number') {
//      steps {
  stage "Create build output" {
     steps {
    
    // Make the output directory.
    sh "mkdir -p output"

    // Write an useful file, which is needed to be archived.
    writeFile file: "output/usefulfile.txt", text: "This file is useful, need to archive it."

    // Write an useless file, which is not needed to be archived.
    writeFile file: "output/uselessfile.md", text: "This file is useless, no need to archive it."

    stage "Archive build output"
    
    // Archive the build output artifacts.
    archiveArtifacts artifacts: 'output/*.txt', excludes: 'output/*.md'
//        sh "touch {pman_version.cur,pman_version.old}"
//        writeFile file: "output/pman_version.txt", text: ${env.BUILD_ID}"
/*        sh "cat pman_version.cur"
        sh "cp pman_version.cur pman_version.old"
        sh "cat pman_version.old"
        sh "echo ${env.BUILD_ID} > pman_version.cur"
        sh "cat pman_version.cur"
*/      }
    }
 }
}
