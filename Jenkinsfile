pipeline{
agent any
stages{
 stage('checkout'){
  steps{ checkout scm}
 }
 stage('Build'){
  steps{ 
  sh 'mvn clean package'
  }
 }
 stage('Build docker image'){
  steps{ 
  sh 'docker build -t fullstackapp:latest .'
  }
 }

}



}