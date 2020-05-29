def service = "trial"
def authkey = "f486be3b-4af4-4514-b9eb-0f639c4aecbb"
def s3bucket = "services-version"
def service_version_cur
def service_version_old
pipeline {
    agent any
    stages{
     stage('Docker push'){
         steps{
             script{
                 docker.withRegistry('https://280064746148.dkr.ecr.eu-west-1.amazonaws.com', 'ecr:eu-west-1:$authkey') {
                 echo "connection established"
             }
           }
        }
    }
  }
}
/*    stages {
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
                        service_version_cur = sh(script: "cat ~/versionfiles/${service}-versionfiles/*_version.cur" , returnStdout: true).trim()
                        service_version_old = sh(script: "cat ~/versionfiles/${service}-versionfiles/*_version.old" , returnStdout: true).trim()
                        echo "printing version files"
                        println(service_version_cur)
                        println(service_version_old)
                }
          }
     }
      stage('deployment') {
          steps {
                script {
                    echo "deployment succedded"
                }
          }
     }
  }
    post {
    success {
        echo "${service} with ${service_version_cur} version deployed successfully"
        }
    failure {
             build job: '../trial/master'
     }
   }
}
*/
