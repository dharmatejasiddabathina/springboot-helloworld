pipeline {
    agent any
       /* envirnoment{
             // path = 'D:\apache-maven-3.8.1:$path'
                   }*/
	stages{
		stage('checkout'){
			steps {
				deleteDir()
				checkout scm

			     }
		                  }
                stage('Build'){
			steps {
                                script{
                                     bat '''
                                     
                                        set M2_HOME="D:/apache-maven-3.8.1"
                                        set path="D:/apache-maven-3.8.1/bin";%path%
				        
                                        mvn clean install
                                        
                                        '''
                                }  

			     }
		                  }
                stage('SonarQube analysis') { 
                       

                        steps{
                                script{
                        
                  withSonarQubeEnv('sonar') {
                                        
                    // Optionally use a Maven environment you've configured already
					
                    withMaven(maven:'maven-3.8.1'){
					    
						
                            
                             
                             bat 'mvn clean package sonar:sonar'
                            
							
                    }
                                }
                        }
    }
	      }
		  
		  stage("Quality Gate"){
		             steps{
					 script{
                 timeout(time: 10, unit: 'MINUTES') { // Just in case something goes wrong, pipeline will be killed after a timeout
                  def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
                 if (qg.status != 'OK') {
                 error "Pipeline aborted due to quality gate failure: ${qg.status}"
                                        }
	                                           }
	                          }
							 }
							}
		
		  
        }
}
