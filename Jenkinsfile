pipeline {
  agent any 
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                echo "PATH = ${PATH}"
                echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
  
   stage ('Security monitoring and misconfigurations') {
        steps {
             sh './securityhub.sh'
            }
      }
    }  
