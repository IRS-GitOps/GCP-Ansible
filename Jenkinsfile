pipeline {
  agent {
    kubernetes {
      yamlFile 'ansible-pod.yml'
    }
  }  
  options { 
    buildDiscarder(logRotator(numToKeepStr: '2'))
    preserveStashes(buildCount: 2)
  }
  stages('VM Creation GCE/Java App Deployment')
  {
    stage('GCE Provisioning/App Deploy PreProd') {
      //when {
       // branch 'preprod'
      //}
      steps {
              container('ansible') {
                checkout scm
                withCredentials([usernamePassword(credentialsId: 'AWX', passwordVariable: 'nexpass', usernameVariable: 'nexuser'), string(credentialsId: 'gcpuserproj', variable: 'gcpuserproj')]) {
                sh 'ansible-playbook -i inventory playbook.yml --tags "deploy" --extra-vars "instance_name=pre-prod nexus_user=${nexuser} nexus_password=${nexpass} target_user=${gcpuserproj} gcp_project=${gcpuserproj}"'
                }
              }
      }
    }  
    stage('GCE Provisioning/App Deploy PROD') {
      when {
        branch 'master'
      }
      steps {
              container('ansible') {
                checkout scm
                withCredentials([usernamePassword(credentialsId: 'AWX', passwordVariable: 'nexpass', usernameVariable: 'nexuser'), string(credentialsId: 'gcpuserproj', variable: 'gcpuserproj')]) {
                sh 'ansible-playbook -i inventory playbook.yml --tags "deploy" --extra-vars "instance_name=prod nexus_user=${nexuser} nexus_password=${nexpass} target_user=${gcpuserproj} gcp_project=${gcpuserproj}"'
                }
              }
      }
    }
  }
}
