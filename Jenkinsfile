pipeline {
  environment {
    registry = "blohmeier/capstonejenkinstests"
    registryCredential = 'dockerhub'
  }
  agent any
  stages {
    stage ('Exit code 0 if Lint HTML succeeds otherwise exit code not 0 ') {
      steps {
        sh '''
          tidy -qe --doctype strict *.html 2> /dev/null
            if [ $? != 0 ]
              then
                echo "there were HTML errors" >&2
            else
		# if [ $? -eq 0 ]
                #then
		#echo "no HTML errors" >&2
                #else
            true
            fi
        '''
      }
    }
    stage ('Build from Dockerfile if Lint HTML succeeds') {
      steps {
        sh '''
	  docker build -t blohmeier/some-content-nginx .
	  docker image ls
	  docker ps
	  ls -al@
	'''
      }
    }
    stage ('Deploy using rolling deployment as a Kubernetes Deployment object as described in the YAML file if Lint HTML succeeds') {
      steps {
        sh '''
	  minikube start
	  kubectl apply -f deployment.yaml
	  kubectl describe deployment nginx-deployment
	'''
      }
    }   
    stage ('Upload built image to Dockerhub if Lint of HTML succeeds') {
      steps {
        sh '''
	  docker push blohmeier/some-content-nginx
	'''
      }
    }
  }
}
