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
  script{
  try{
  sh 'docker build -t fullstackapp:latest .'
  }
  catch (err){
  currentBuild.error = 'FAILURE'}
  }
  }
 }

}
post{
 failure{
 Cleanws()
 }

}



}