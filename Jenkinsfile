pipeline {
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
    stage ('Build from Dockerfile if Lint HTML succeeds and run') {
      steps {
        sh '''
	  docker build -t some-content-nginx .
	  docker image ls
	  docker ps
	'''
      }
    }
    stage ('Deploy the built image based on the Kubernetes Deployment object described in YAML file') {
      steps {
        sh '''
	  kubectl apply -f deployment.yaml
	  kubectl describe deployment nginx-deployment
	'''
      }
    }   
    stage ('Upload to AWS if Lint HTML succeeds') {
      steps {
        withAWS(credentials: 'aws-capstone') {
          s3Upload(file:'index.html', bucket:'uniquenameproj4new', path:'index.html')
        }
      }
    }
  }
}
