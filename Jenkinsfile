pipeline {
  agent any 
  tools {
     maven 'M2_HOME'
        }
stages {
     stage('Git Checkout') {
       steps {
         git 'https://github.com/devopscbabu/24-Apr-Insureme.git'
             }
        }
     stage('Build Package') {
       steps {
         sh 'mvn package'
       }
     }
     stage('Publish HTML Reports') {
       steps {
         publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/insurance/target/', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
             }
         }

  stage('Create Docker image of App') {
       steps {
         sh 'docker build -t gopala230390/insurance:3.0 .'
             }
         }

    stage('Docker Image Push') {
       steps {
       withCredentials([usernamePassword(credentialsId: 'dockerpass', passwordVariable: 'dockerpass', usernameVariable: 'dockerhub')]) {
         sh 'docker login -u ${dockerhub} -p ${dockerpass}'
       }
         sh 'docker push gopala230390/insurance:3.0'
   }    
     }   
  
  stage('Application Deploy-container') {
          steps {
            
        ansiblePlaybook credentialsId: 'ubuntu-ssh', disableHostKeyChecking: true, installation: 'ansible', inventory: 'prod.inv', playbook: 'deploy.yml'
                }
          }

    
}
}
