def service = "trial"
def authkey = "f486be3b-4af4-4514-b9eb-0f639c4aecbb"
def s3bucket = "services-version"

pipeline {
    agent any
    stages {
      stage('Create verison files and push to s3 bucket') {
          steps {
                script {
                        sh "mkdir ~/${service}-versionfiles"
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
                        echo "pulling version files"
                        version.cur = readFile(file: '~/${service}-versionfiles/${service}_version.cur')
                        version.old = readFile(file: '~/${service}-versionfiles/${service}_version.old')
                        echo "printing version files"
                        println(version.cur)
                        println(version.old)
                }
          }
     }
   }
}
