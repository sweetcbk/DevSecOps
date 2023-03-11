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
        stage ('Check Secrets') {
       steps {
       sh 'trufflehog3 https://github.com/sweetcbk/DevSecOps.git -f json -o truffelhog_output.json || true'
       }
     }
      stage ('Software Composition Analysis') {
             steps {
                 dependencyCheck additionalArguments: ''' 
                     -o "./" 
                     -s "./"
                     -f "ALL" 
                     --prettyPrint''', odcInstallation: 'owasp-dc'

                 dependencyCheckPublisher pattern: 'dependency-check-report.xml'
             }
         }
   
stage ('Static Analysis') {
      steps {
        withSonarQubeEnv('Sonar') {
          sh 'mvn sonar:sonar'
            
        }
      }
    }
      
      stage ('Deploy to Server Application') {
            steps {
           sshagent(['server-application']) {
              sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/project/webgoat-server-v8.2.0-SNAPSHOT.jar ubuntu@3.89.194.15:/WebGoat'
             sh 'ssh -o  StrictHostKeyChecking=no ubuntu@3.89.194.15 "nohup java -jar webgoat-server-v8.2.0-SNAPSHOT.jar --server.address=0.0.0.0 --server.port=8080 &"'
    
           }
           }     
        }
      stage ('Dynamic Analysis') {
            steps {
           sshagent(['server-application']) {
               sh 'ssh -o  StrictHostKeyChecking=no ubuntu@34.228.38.88 "sudo ./zap.sh -cmd -quickurl http://3.89.194.15:8080/WebGoat -quickprogress -quickout ~/Aut.xml || true" '
           }      
           }       
    }
      stage ('Host Vulnerability Assessment') {
       steps {
            sh 'echo "In-Progress"'
            }
   }
     
     }
    }  
