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
        
    /* stage('SonarQube analysis'){ 
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
          }*/
        
        stage('Uploading artifacts to Ansible'){
            steps{
                   //withCredentials([string(credentialsId: 'ANSADMIN_PASSWORD', variable: 'ansadmin_password')]){
     //sh 'sshpass -p ${ansadmin_password} ssh -v -o StrictHostKeyChecking=no ansadmin@172.31.36.158 \"cd /home/ansadmin/target; wget -O sfinder-0.0.1-SNAPSHOT.war http://18.219.109.108:8080/var/lib/jenkins/workspace/springboot-finder/target/sfinder-0.0.1-SNAPSHOT.war \"' 
 withCredentials([usernamePassword(credentialsId: 'ansible', passwordVariable: 'pass', usernameVariable: 'userId')]) {
                //sh 'curl -v -F file=@target/sfinder-0.0.1-SNAPSHOT.war -u ${userId}:${pass} ansadmin@172.31.36.158/etc/ansible/playbooks/target/sfinder-0.0.1-SNAPSHOT.war'
                              sh 'sshpass -p ${pass} scp -o StrictHostKeyChecking=no target/*.war ansadmin@172.31.36.158:~/target/'


            }
}
        }
        stage('Uploading playbook to Ansible'){
            steps{
                 withCredentials([usernamePassword(credentialsId: 'ansible', passwordVariable: 'pass', usernameVariable: 'userId')]) {
                //sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '//opt//playbooks', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'project-ansible.yml')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])   
                sh 'sshpass -p ${pass} scp -v playbook.yml ansadmin@172.31.36.158:/opt/playbooks'
                      //sh 'sshpass -p ${pass} scp -o StrictHostKeyChecking=no target/*.war ansadmin@172.31.36.158:~/target/'

                }
            }
            
        }
        stage('Executing Playbook'){
            steps{
               withCredentials([usernamePassword(credentialsId: 'ansible', passwordVariable: 'pass', usernameVariable: 'userId')]) {
                    sh 'sshpass -p ${pass} ssh -v -o StrictHostKeyChecking=no ansadmin@172.31.36.158 \"ansible-playbook /opt/playbooks/playbook.yml\"'
                    //sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible_server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'ansible-playbook /opt/playbooks/project-ansible.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: true)])
                }
            }
        }
   
}
}
