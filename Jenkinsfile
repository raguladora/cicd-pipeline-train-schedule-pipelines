def service = "trial"
def authkey = "f486be3b-4af4-4514-b9eb-0f639c4aecbb"
def s3bucket = "services-version"

pipeline {
    agent any
    stages {
      stage('Create verison files and push to s3 bucket') {
          steps {
                script {
                        sh "touch ~/${service}-versionfiles/${service}_version.cur ~/${service}-versionfiles/${service}_version.old"
                        sh "mv ~/${service}-versionfiles/${service}_version.cur ~/${service}-versionfiles/${service}_version.old"
                        sh "echo ${env.BUILD_ID} > ~/${service}-versionfiles/${service}_version.cur"
//                      withAWS(credentials:"$authkey") {
                                // sh "aws s3 cp ~/${service}-versionfiles/${service}_version.cur s3://${s3bucket}/${service}/${service}_version.cur"
                                // sh "aws s3 cp ~/${service}-versionfiles/${service}_version.old s3://${s3bucket}/${service}/${service}_version.old"
 //                       }
                }
          }
     }
      stage('pull version files from s3 bucket and assign to a variable') {
          steps {
                script {
//                      withAWS(credentials:"$authkey") {
                                // sh "aws s3 cp s3://${s3bucket}/${service}/${service}_version.cur ~/${service}-versionfiles/${service}_version.cur"
                                // sh "aws s3 cp s3://${s3bucket}/${service}/${service}_version.cur ~/${service}-versionfiles/${service}_version.old "
//                        }
                        echo "reading version files"
                        sh "ls -all ~/${service}-versionfiles/"
                        service_version1.cur = readFile(file: '/var/lib/jenkins/trial-versionfiles/trial_version.old')
                        service_version1.old = readFile(file: '/var/lib/jenkins/trial-versionfiles/trial_version.old')
                        service_version.cur = sh(script: cat '/var/lib/jenkins/trial-versionfiles/trial_version.cur' , returnStdout: true)
                        service_version.old = sh(script: cat '/var/lib/jenkins/trial-versionfiles/trial_version.old' , returnStdout: true)
                        echo "printing version files"
                        println(service_version1.cur)
                        println(service_version1.old)
                        println(service_version.cur)
                        println(service_version.old)

                }
          }
     }
   }
}
