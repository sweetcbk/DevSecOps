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
        stage ('Check secrets') {
       steps {
       sh 'trufflehog3 https://github.com/sweetcbk/secirity.git -f json -o truffelhog_output.json || true'
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
   
 stage ('Static analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh 'mvn sonar:sonar'
	}
      }
    }
      
//      stage ('Deploy to server-application') {
//            steps {
//           sshagent(['server-application']) {
//              sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/project/webgoat-server-v8.2.0-SNAPSHOT.jar ubuntu@107.21.174.136:/WebGoat'
//             sh 'ssh -o  StrictHostKeyChecking=no ubuntu@107.21.174.136 "nohup java -jar webgoat-server-v8.2.0-SNAPSHOT.jar --server.address=0.0.0.0 --server.port=8080 &"'
//    
//           }
//           }     
//        }
//      stage ('Dynamic analysis') {
//            steps {
//           sshagent(['server-application']) {
//                sh 'ssh -o  StrictHostKeyChecking=no ubuntu@54.173.19.17 "sudo ./zap.sh -cmd -quickurl http://107.21.174.136:8080/WebGoat -quickprogress -quickout ~/Aut.xml || true" '
//             }      
//           }       
//    }
//      stage ('Host vulnerability assessment') {
//        steps {
//             sh 'echo "In-Progress"'
//            }
//    }
     
     }
    }  
