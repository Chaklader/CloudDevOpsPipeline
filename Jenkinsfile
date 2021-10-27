pipeline {
	
    agent any
	
    stages {
	    
        stage('Step #1: Lint HTML') {
            steps {
		sh 'echo "Lint check..."'
                sh 'tidy -q -e *.html'
            }
        }

        stage('Step #2: Lint Dockerfile') {
            steps {
                sh 'hadolint ./Dockerfile | tee -a hadolint_lint.txt'
                sh '''
                        lintErrors=$(stat --printf="%s"  hadolint_lint.txt)

                        if [ "$lintErrors" -gt "0" ]; then
                            echo "Errors have been found, please see below"
                            cat hadolint_lint.txt
                            exit 1
                        else
                            echo "There are no erros found on Dockerfile!!"
                        fi
                '''
            }
        }
	    
	// stage('Build Docker Image') {
   	//     steps {
        //         withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
		    
        //             sh 'echo "Building Docker Image..."'
     	//     	    sh 'docker build -t chaklader/capstoneapp .'
	// 	}
        //     }
        // }
	    
	// stage('Push Image To Dockerhub') {
   	//     steps {
        //         withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]){
	// 	    sh 'echo "Pushing Docker Image..."'

     	//     	    sh '''
        //                 docker login -u $USERNAME -p $PASSWORD
	// 		docker push chaklader/capstoneapp
        //             '''
	// 	}
        //     }
        // }
	 
	// stage('Create k8s cluster') {
	//     steps {
	// 	withAWS(credentials: 'aws-kubectl', region: 'us-west-1a') {
	// 	    sh 'echo "Create k8s cluster..."'
	// 	    sh '''
	// 		/usr/local/bin/eksctl create cluster \
	// 		--name capstoneappprojectffff \
	// 		--version 1.17 \
	// 		--region us-west-1 \
	// 		--nodegroup-name standard-workers \
	// 		--node-type t2.micro \
	// 		--nodes 2 \
	// 		--nodes-min 1 \
	// 		--nodes-max 3 \
	// 		--managed
	// 	'''
	// 	}
	//     }
        // }
	    
	// stage('Configure kubectl') {
	//     steps {
	// 	withAWS(credentials: 'aws-kubectl', region: 'us-west-1a') {
	// 	    sh 'echo "Configure kubectl..."'
	// 	    sh '/usr/local/bin/aws eks --region us-west-1 update-kubeconfig --name capstoneappprojectffff' 
	// 	}
	//     }
        // }

	// stage('Deploy blue container') {
	//     steps {
	// 	withAWS(credentials: 'aws-kubectl', region: 'us-west-1a') {
	// 	    sh 'echo "Deploy blue container..."'
	// 	    sh '/usr/local/bin/kubectl apply -f ./blue/blue.yaml'
	// 	}
	//     }
	// }
	    	    
	// stage('Deploy green container') {
	//     steps {
	// 	withAWS(credentials: 'aws-kubectl', region: 'us-west-1a') {
	// 	    sh 'echo "Deploy green container..."'
	// 	    sh '/usr/local/bin/kubectl apply -f ./green/green.yaml'
	// 	}
	//     }
	// }
	    
	// stage('Create blue service') {
	//     steps {
	// 	withAWS(credentials: 'aws-kubectl', region: 'us-west-1a') {
	// 	    sh 'echo "Create blue service..."'
	// 	    sh '/usr/local/bin/kubectl apply -f ./blue/blue_service.yaml'
	// 	}
	//     }
	// }
	    
	// stage('Update service to green') {
	//     steps {
	// 	withAWS(credentials: 'aws-kubectl', region: 'us-west-1a') {
	// 	    sh 'echo "Update service to green..."'
	// 	    sh '/usr/local/bin/kubectl apply -f ./green/green_service.yaml'
	// 	}
	//     }
	// }	
   }   
	    
}
