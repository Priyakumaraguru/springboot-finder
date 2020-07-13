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
        
     /*stage('SonarQube analysis'){ 
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
                              sh 'sshpass -p ${pass} scp -o StrictHostKeyChecking=no target/*.war -u ${userId}:${pass} ansadmin@172.31.36.158:~/target/'

            }
}
        }
   stage('deploy')
              {
                  steps
                  {
                      deploy adapters: [tomcat9(credentialsId: '7bba93f5-d5bd-427e-b462-f3c552dad961', path: '', url: 'http://3.128.188.221:8090/')], contextPath: '/finder', war: '**/*.war'
                      //deploy adapters: [tomcat9(credentialsId: '7bba93f5-d5bd-427e-b462-f3c552dad961', path: '', url: 'http://52.14.6.176:8090/')], contextPath: '/cur', war: '**/*.war'
                  }  
              }   
}
}
