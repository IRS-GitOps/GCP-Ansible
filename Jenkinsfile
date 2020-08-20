pipeline {
  agent {
    kubernetes {
      yamlFile 'ansible-pod.yml'
    }
  }  
  options { 
    buildDiscarder(logRotator(numToKeepStr: '2'))
    skipDefaultCheckout true
    preserveStashes(buildCount: 2)
  }
  stages('VM Creation GCE/Java App Deployment')
  {
    stage('GCE Provisioning/App Deploy PreProd') {
      when {
        branch 'preprod'
      }
      steps {
              container('ansible') {
                checkout scm
                sh'ansible-playbook -i inventory/ playbook.yml --tags "deploy"'
              }
      }
    }  
    stage('GCE Provisioning/App Deploy PROD') {
      //when {
       // branch 'master'
      //}
      steps {
              container('ansible') {
                checkout scm
                sh'ansible-playbook -i inventory/ playbook.yml --tags "deploy"' 
              }
      }
    }
  }
}
