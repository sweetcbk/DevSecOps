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
//        stage ('Check secrets') {
//       steps {
//       sh 'trufflehog3 https://github.com/sweetcbk/secirity.git -f json -o truffelhog_output.json || true'
//       }
//     }
//      stage ('Software Composition Analysis') {
//             steps {
//                 dependencyCheck additionalArguments: ''' 
//                     -o "./" 
//                     -s "./"
//                     -f "ALL" 
//                     --prettyPrint''', odcInstallation: 'owasp-dc'

//                 dependencyCheckPublisher pattern: 'dependency-check-report.xml'
//             }
//         }
   
//  stage ('Static Application Security Testing') {
// 	      steps {
//         	withSonarQubeEnv('SonarQubeScanner') {
// 	          sh 'mvn sonar:sonar'
// 				}
// 	      	}
//     	}
      
      stage ('Deploy to server-application') {
            steps {
           sshagent(['server-application']) {
                sh 'scp -o StrictHostKeyChecking=no /var/lib/jenkins/workspace/project/webgoat-server-v8.2.0-SNAPSHOT.jar ubuntu@54.147.56.117:/WebGoat'
               sh 'ssh -o  StrictHostKeyChecking=no ubuntu@54.147.56.117 "nohup java -jar webgoat-server-v8.2.0-SNAPSHOT.jar --server.address=0.0.0.0 --server.port=8080 &"'
    
           }
           }     
        }
      stage ('Dynamic analysis') {
            steps {
           sshagent(['application_server']) {
                sh 'ssh -o  StrictHostKeyChecking=no ubuntu@18.205.235.122 "sudo docker run --rm -v /home/ubuntu:/zap/wrk/:rw -t owasp/zap2docker-stable zap-full-scan.py -t http://54.147.56.117:8080/WebGoat -x zap_report || true" '
		sh 'ssh -o  StrictHostKeyChecking=no ubuntu@18.205.235.122 "sudo ./zap_report.sh"'
              }      
           }       
    }
      
      
     }
    }  

