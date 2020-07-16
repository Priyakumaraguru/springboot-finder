pipeline {
    agent any
 tools{
    maven 'maven'
    }
    stages {

      stage('clean')
            {
                steps
                 { 
                    sh 'mvn clean install'
                 }
            }
        
     stage('SonarQube analysis'){ 
        environment{
               scannerHome = tool 'Sonar'
                   }
       steps{
        withSonarQubeEnv('SonarQube') {
            sh "${scannerHome}/bin/sonar-scanner"
         
           }
       }
     }
        stage("Quality Gate") {
            steps {
              timeout(time: 10, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
            }
          }
        
       stage('Uploading artifacts to Ansible'){
            steps{
 withCredentials([usernamePassword(credentialsId: 'ansible', passwordVariable: 'pass', usernameVariable: 'userId')]) {
                              sh 'sshpass -p ${pass} scp -o StrictHostKeyChecking=no target/*.war ansadmin@172.31.36.158:~/target/'

            }
}
        }
        stage('Uploading playbook to Ansible'){
            steps{
                 withCredentials([usernamePassword(credentialsId: 'ansible', passwordVariable: 'pass', usernameVariable: 'userId')]) {  
                sh 'sshpass -p ${pass} scp -v playbook.yml ansadmin@172.31.36.158:~/playbooks'

                }
            }
            
        }
        stage('Executing Playbook'){
            steps{
               withCredentials([usernamePassword(credentialsId: 'ansible', passwordVariable: 'pass', usernameVariable: 'userId')]) {
                    sh 'sshpass -p ${pass} ssh -v -o StrictHostKeyChecking=no ansadmin@172.31.36.158 \"ansible-playbook /home/ansadmin/playbooks/playbook.yml\"'
                }
            }
        }
       
   
}
}
