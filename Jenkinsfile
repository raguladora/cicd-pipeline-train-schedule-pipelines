def authkey
def servicepath
def host
def port
def username = "djadmin"
def password
def dbname


pipeline {
  agent any

  parameters {
    string(name: 'BUILD_PATH', defaultValue: 'prod-ecs-pub-pri/databases/', description: 'path from which terraform deployment needs to be run')
    string(name: 'BRANCH_NAME', defaultValue: "", description: "value should be either of dev','uat','seuat','uatuk','prod','devops','demotest16','TEMPLATE'")
    choice(name: 'SERVICE', choices: ['pman','postcode','sso','wlog'], description: 'Choose service')
}

  stages {

    stage('Deploy To Environment'){
      steps {
        input  "Do you want to proceed for deployment?"
      }
    }
    stage('setting database credentials') {
      steps {
        script {
          if( params.SERVICE == 'postcode' ) {
            host = "postcode-devops.c8irfgmotewk.eu-west-1.rds.amazonaws.com"
            port = "3306"
          }
          else {
            host = "${params.SERVICE}${params.BRANCH_NAME}-db.djaplatform.com"
            port = "5432"
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
        sh "mkdir -p ${params.SERVICE}/${dbname}"
        dir("${params.SERVICE}/${dbname}"){
          sh "export PGPASSWORD=${password}"
          sh "pg_dump -h ${host} -p ${port} -U ${username} -d ${dbname} > ${dbname}-${BUILD_TIMESTAMP}.sql"
          sh "cat ${dbname}-${BUILD_TIMESTAMP}.sql"
        }
        sh "ls -all ${params.SERVICE}/${dbname}"
      }
    }
  }
}
