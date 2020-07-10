def authkey
def servicepath
def host = "ssodevops-db.djaplatform.com"
def port
def username = "djadmin"
def password
def dbname
def s3bucket = "apropos-dev-tf-state"
def latestbackupfile

pipeline {
  agent any

  parameters {
    string(name: 'BUILD_PATH', defaultValue: 'prod-ecs-pub-pri/maintenance/', description: 'path from which terraform deployment needs to be run')
    string(name: 'BUILD_DB_PATH', defaultValue: 'prod-ecs-pub-pri/databases/', description: 'path from which terraform deployment needs to be run')
    string(name: 'BRANCH_NAME', defaultValue: "", description: "value should be either of dev','uat','seuat','uatuk','prod','devops','demotest16','TEMPLATE'")
    choice(name: 'SERVICE', choices: ['pman','postcode','sso','wlog'], description: 'Choose service')
    choice(name: 'ACTION', choices: ['plan','show','apply','destroy','output'], description: 'Image name to publish')
}

  stages {

     stage('Deploy To Environment'){
       steps {
         script {
           if( !( params.BRANCH_NAME == 'devops' ||  params.BRANCH_NAME == 'prod' || params.BRANCH_NAME == 'uat' || params.BRANCH_NAME == 'seuat' || params.BRANCH_NAME == 'devops' || params.BRANCH_NAME == 'DEVTEMPLATE' || params.BRANCH_NAME == 'uatuk' || params.BRANCH_NAME == 'training' || params.BRANCH_NAME == 'PRODTEMPLATE' || params.BRANCH_NAME == 'devops') ) {
             error("Invalid environment name ${params.BRANCH_NAME}")
           }
         }

         input  "Do you want to proceed for deployment?"

      }
  }
    stage('Prepare environment configuration') {
      steps {
        script {

          if( params.BRANCH_NAME == 'prod' || params.BRANCH_NAME == 'uat' || params.BRANCH_NAME == 'seuat' || params.BRANCH_NAME == 'uatuk' || params.BRANCH_NAME == 'training' || params.BRANCH_NAME == 'PRODTEMPLATE' ) {
            authkey = "085a26c5-6009-4dc2-ab6d-b1aae52ffb5a"
          }
          else {
            authkey = "f486be3b-4af4-4514-b9eb-0f639c4aecbb"
          }
        }
        sh "mkdir -p ${params.BRANCH_NAME}/${params.BUILD_PATH}"
        sh "cp -av ${params.BUILD_PATH}/* ${params.BRANCH_NAME}/${params.BUILD_PATH}/"
        sh "cp -av common ${params.BRANCH_NAME}/"
        dir("${params.BRANCH_NAME}/${params.BUILD_PATH}") {
          sh "${WORKSPACE}/common/scripts/envconfig.sh ${params.BRANCH_NAME}"
        }
      }
    }
    stage('Set Terraform details and Initialization') {
      steps {
        echo 'Set Terraform details and Initialization'
      }
    }

    stage('Enabling Maintenance Mode') {
      steps {
        echo 'enabling maintenance Mode'
      }
    }
    stage('Waiting for maintenance activity to complete') {
      steps {
        input "Do you want to come out of maintenance mode?"
      }
    }
    stage('Disabling Maintenance Mode ') {
      steps {
        echo 'Set Terraform details and Initialization'
      }
    }
    stage ("waiting for 10seconds") {
     steps {
      sleep 10
     }
    }
    stage('Prepare Environment, Set Terraform details and Initialization on DATABASE directory') {
      steps {
        script {
         sh "echo ${authkey}"
         servicepath = "${params.BUILD_DB_PATH}/${SERVICE}"
        }
        sh "mkdir -p ${params.BRANCH_NAME}/${servicepath}"
        sh "cp -av ${servicepath}/* ${BRANCH_NAME}/${servicepath}/"
        sh "cp -av common ${params.BRANCH_NAME}/"
        dir("${params.BRANCH_NAME}/${servicepath}") {
          sh "${WORKSPACE}/common/scripts/envconfig.sh ${params.BRANCH_NAME}"
        }
        echo 'terraform version'
        dir("${params.BRANCH_NAME}/${servicepath}") {
          withAWS(credentials:"$authkey") {
            echo 'terraform init'
          }
        }
      }
    }
    stage('Provision infrastructure for output') {
      steps {
        dir("${params.BRANCH_NAME}/${servicepath}")
        {
          withAWS(credentials:"$authkey") {
            echo "if [ '${params.ACTION}' = 'apply' ] || [ '${params.ACTION}' = 'destroy' ] || [ '${params.ACTION}' = 'refresh' ]; then terraform ${params.ACTION} -auto-approve -lock=false -no-color; elif [ '${params.ACTION}' = 'plan' ]; then terraform ${params.ACTION} -no-color ; else terraform ${params.ACTION} -no-color; fi"
          }
        }
      }
    }
    stage('setting database credentials') {
      steps {
        script {
           if( params.SERVICE == 'postcode' ) {
            port = "3306"
           }
           else {
            port = "5432"
           }
          }
        script {
         if( params.ACTION == 'output' ) {
         dir("${params.BRANCH_NAME}/${servicepath}"){
           withAWS(credentials:"$authkey") {
//            host = sh (
//              script: "terraform '${params.ACTION}' -no-color | cut -d' ' -f3",
//              returnStdout: true
//              ).trim()
           }
          }
         }
        }
        script {
          if( params.SERVICE == 'pman' ) {
            dbname = "pmanager${params.BRANCH_NAME}"
          }
          else {
            dbname = "${params.SERVICE}${params.BRANCH_NAME}"
          }
        }
        script {
          if ( params.BRANCH_NAME == 'uatuk' ) {
            password = "55y3KVmu816V"
          }
          else {
            password = "vEhW9X1vxB"
         }
       }
     }
    }
    stage('dumping database') {
      steps {
       sh "echo ${host}"
       sh "echo ${port}"
       sh "echo ${password}"
       sh "echo ${dbname}"
       sh "mkdir -p ${params.BRANCH_NAME}/${servicepath}/${dbname}"
       dir("${params.BRANCH_NAME}/${servicepath}/${dbname}"){
         sh "if [ '${params.SERVICE}' = 'postcode' ]; then  mysqldump -h ${host} -P ${port} -u ${username} -d ${dbname} -p${password} ${dbname} > ${dbname}.sql;  else export PGPASSWORD=${password} && pg_dump -h ${host} -p ${port} -U ${username} -d ${dbname} > ${dbname}.sql; fi"
        }
        sh "ls -all ${params.BRANCH_NAME}/${servicepath}/${dbname}"
      }
    }
    stage('copy backup file to s3') {
     steps {
      script {
        dir("${params.BRANCH_NAME}/${servicepath}/${dbname}") {
          withAWS(credentials:"$authkey") {
           sh "aws s3 cp ${dbname}.sql s3://${s3bucket}/${params.BRANCH_NAME}/backups/${params.SERVICE}/${dbname}-${BUILD_TIMESTAMP}.sql"
           sh "rm -rf ${dbname}.sql"
          }
        }
      }
     }
    }

  }
//    post {
//        always {
//                 emailext body: 'Please find build report attached: <pre>${BUILD_LOG, maxLines=2000}</pre> ${BUILD_URL}console ', recipientProviders: [developers()], subject: " Build #${currentBuild.number} , ${env.JOB_NAME} , ${currentBuild.currentResult} ", to: 'techteamindia@apropos.app'
//               }
//     }
}

