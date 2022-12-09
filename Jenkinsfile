properties([pipelineTriggers([githubPush()])])

pipeline {
  agent {
    kubernetes {
     yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: maven
            image: maven:alpine
            command:
            - cat
            tty: true
          - name: docker
            image: docker:latest
            command:
            - cat
            tty: true
            volumeMounts:
             - mountPath: /var/run/docker.sock
               name: docker-sock
          volumes:
          - name: docker-sock
            hostPath:
              path: /var/run/docker.sock
        '''
       }
   }
	
  stages {
   stage('Clone') {
      steps {
        container('maven') {
          git branch: 'master', changelog: false, poll: false, url: 'https://github.com/rachit89/k8s-mastery.git'
        }
      }
    }
	stage('Build-Docker-Image') {
      steps {
        container('docker') {
		  script{
          echo "Test code from github"
          sh  '''
          ## echo "Rachit@2050"| docker login --username rachit22 --password-stdin
          cd sa-frontend
          docker build -t frontapp .
          docker tag frontapp rachit22/frontapp-test:latest-${BUILD_NUMBER}
          ##docker push rachit22/frontapp-test:latest-${BUILD_NUMBER}
          cd ../sa-logic/
          docker build -t logicapp .
          docker tag logicapp rachit22/logicapp-test:latest-${BUILD_NUMBER}
          ##docker push rachit22/logicapp-test:latest-${BUILD_NUMBER}
          cd ../sa-webapp/
          docker build -t webapp .
          docker tag webapp rachit22/webapp-test:latest-${BUILD_NUMBER}
          ##docker push rachit22/webapp-test:latest-${BUILD_NUMBER}
          '''
        }
    stage('Docker Push') {
      steps {
        container('docker') {
		script{
	  sh '''
      	withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        	sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
            docker push rachit22/frontapp-test:latest-${BUILD_NUMBER}
            docker push rachit22/logicapp-test:latest-${BUILD_NUMBER}
            docker push rachit22/webapp-test:latest-${BUILD_NUMBER}
	    '''
		}
	  
        }
       }
     }
    }		  
        }
      }
    }
}
