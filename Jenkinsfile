   
def gv
pipeline {
   agent any
     parameters {
	    choice (name: 'VERSION', choices: ['1.0.0', '1.0.1', '1.0.2'], description: 'Deployment of selected version')
	        booleanParam(name: 'appBuild', defaultValue: true, description: '')
		booleanParam(name: 'appTesting', defaultValue: false, description: '')
	        booleanParam(name: 'dockerBuild', defaultValue: true, description: '')
	        booleanParam(name: 'dockerPush', defaultValue: true, description: '')
	        booleanParam(name: 'remoteServerDeploy', defaultValue: false, description: '')
	        booleanParam(name: 'awsEcsDeploy', defaultValue: false, description: '')
	        booleanParam(name: 'dockerClean', defaultValue: true, description: '')
		}
	environment {
		DOCKERRUN = "docker run -p 8080:8080 my-app binueuginc/sample-myapp:${params.VERSION} " 
		DOCKERCLEAN = "docker images  | awk '{print \$1 \":\" \$2}' | xargs docker rmi -f"
	}
         stages {
	    stage('init'){
	     steps {
		   script {
		   gv = load "groovy.script"
		      } 
		   }
	    }
		
	   stage('Appbuild') {
	      when {
		     expression {
			     params.appBuild == true 
				}
			}	
	      steps {
		      script {
			      gv.appBuild()
		      }
		
		  }
		}
	 stage('appTest') {
	      when {
		     expression {
			     params.appTesting == true 
				}
			}	
	      steps {
		      script {
			      gv.appTesting()
		      }
		
		  }
		}
		stage('dockerBuild'){
		   when {
		      expression {
			     params.dockerBuild == true 
				 }
		   }
		   steps{
			   script {
		               gv.dockerBuild()
			 }
		     }
		}
		 stage('dockerPush'){
		   when {
		      expression {
			      params.dockerPush == true 
				 }
		    }
		   steps{
			   script {
		               gv.dockerPush()
			   }
                   }
		}
		stage('remoteServerDeploy'){
		   when {
		      expression {
			      params.remoteServerDeploy == true 
				 }
		    }
		   steps{
			   script {
		               gv.dockerRemoteDeployApp()
			   }
             }
		}
		stage('awsEcsDeploy'){
		   when {
		      expression {
			      params.executeAwsEcsDeploy == true 
				 }
		    }
		   steps{
			   script {
		               gv.dockerEcsDeployApp()
			   }
             }
		}
	    stage('dockerImagesCleanUp'){
		when {
		   expression {
			 params.executeDockerClean == true 
			     }
		     }
		   steps{
		      script {
		          gv.dockerCleanup()
			   }
                   }
	    }
     }	

}
