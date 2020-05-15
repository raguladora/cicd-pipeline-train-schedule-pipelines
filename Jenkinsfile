def service = "trial"
def authkey = "f486be3b-4af4-4514-b9eb-0f639c4aecbb"
def s3bucket = "services-version"

pipeline {
    agent any
    stages {
      stage('Create verison files and push to s3 bucket') {
          steps {
                script {
                        sh "mkdir -p ~/versionfiles/${service}-versionfiles"
                        sh "touch ~/versionfiles/${service}-versionfiles/${service}_version.cur ~/versionfiles/${service}-versionfiles/${service}_version.old"
                        sh "mv ~/versionfiles/${service}-versionfiles/${service}_version.cur ~/versionfiles/${service}-versionfiles/${service}_version.old"
                        sh "echo ${env.BUILD_ID} > ~/versionfiles/${service}-versionfiles/${service}_version.cur"
//                      withAWS(credentials:"$authkey") {
                                // sh "aws s3 cp ~/versionfiles/${service}-versionfiles/${service}_version.cur s3://${s3bucket}/${service}/${service}_version.cur"
                                // sh "aws s3 cp ~/versionfiles/${service}-versionfiles/${service}_version.old s3://${s3bucket}/${service}/${service}_version.old"
 //                       }
                }
          }
     }
      stage('pull version files from s3 bucket and assign to a variable') {
          steps {
                script {
//                      withAWS(credentials:"$authkey") {
                                // sh "aws s3 cp s3://${s3bucket}/${service}/${service}_version.cur ~/versionfiles/${service}-versionfiles/${service}_version.cur"
                                // sh "aws s3 cp s3://${s3bucket}/${service}/${service}_version.cur ~/versionfiles/${service}-versionfiles/${service}_version.old "
//                        }
                        echo "reading version files"
                        sh "cd ~/versionfiles/${service}-versionfiles/"
                        sh "ls -all ~/versionfiles/${service}-versionfiles/"
                        sh "pwd"
//                        service_version_cur = sh "cat ~/versionfiles/${service}-versionfiles/*_version.cur"
//                        service_version_old = sh "cat ~/versionfiles/${service}-versionfiles/*_version.cur"
                        service_version_cur = sh(script: "cat ~/versionfiles/${service}-versionfiles/*_version.cur" , returnStdout: true)
                        service_version_old = sh(script: "cat ~/versionfiles/${service}-versionfiles/*_version.old" , returnStdout: true)
                        echo "printing version files"
                        println(service_version_cur)
                        println(service_version_old)
                }
          }
     }
   }
}
