pipeline {
 agent any
 parameters { 
  choice (
    name: 'ENV', choices: ['Dev', 'QA', 'PROD'], description: 'Select environment'
     )
	}
  stages{
   stage('Checkout'){
    steps{
	 checkout scm
	}
   }
   stage('Build'){
    steps{
	  sh 'mvn clean package'
	}
   }
  
  
  
  
  
  }


}  