pipeline{
agent any
environment{
		ACR_LOGIN_SERVER = "devopsproject1.azurecr.io"
		IMAGE_NAME = 'fullstackapp'
		TAG = 'latest'
		}
parameters{
choice(
 name: 'ENV', choices: ['Dev','Test','Prod'], description: 'Select Environment'
)
}
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
 stage('Login to ACR') {
       steps {
         withCredentials([usernamePassword(
             credentialsId: 'acr-creds',
             usernameVariable: 'ACR_USER',
             passwordVariable: 'ACR_PASS'
         )]) {
             sh '''
               echo $ACR_PASS | docker login $ACR_LOGIN_SERVER \
               -u $ACR_USER --password-stdin
             '''
           }
          }
         }
	    stage('Tag Image') {
        steps {
         sh '''
           docker tag ${IMAGE_NAME}:${TAG} \
           $ACR_LOGIN_SERVER/${IMAGE_NAME}:${TAG}
         '''
          }
        }
		stage('Push Image to ACR'){
	     steps{
	    sh 'docker push $ACR_LOGIN_SERVER/${IMAGE_NAME}:${TAG}'
	    }
	   }
	   stage('Approval'){
	    steps{
		input: 'Approve the pipeline', ok: 'Approve'
		}
	   }
	   stage('Deploy to pord'){
		steps{
		 sh '''
		 kubectl apply -f deployment.yml
		 kubectl apply -f ingress.yml
		 '''
		}
	   }

}



}