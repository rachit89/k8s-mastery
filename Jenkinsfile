pipeline {
  agent {
    kubernetes {
      label 'kubeagent'  // all your pods will be named with this prefix, followed by a unique id
      idleMinutes 1  // how long the pod will live after no jobs have run on it
      yamlFile 'build-pod.yaml'  // path to the pod definition relative to the root of our project 
      defaultContainer 'maven'  // define a default container if more than a few stages use it, will default to jnlp container
    }
  }
stages{
  stage('Go to repo'){
    steps{
        script{
          echo "take code from github"
          git branch: 'master',
                credentialsId: 'github',
                url: 'https://github.com/rachit89/k8s-mastery.git'
             echo "Branch copied"
        }
    }
  } 
  }
  stage('Build Image and push to dockerhub'){
    steps{
        script{
          echo "Test code from github"
          sh  ''' 
          echo "dckr_pat_m1YMEVgpd4v_yefgNCtLr5-sZOM"| docker login --username rachit22 --password-stdin
          cd sa-frontend
          docker build -t frontapp .
          docker tag frontapp rachit22/frontapp:latest-${BUILD_NUMBER}
          docker push rachit22/frontapp:latest-${BUILD_NUMBER}
          cd ../sa-logic/
          docker build -t logicapp .
          docker tag logicapp rachit22/logicapp:latest-${BUILD_NUMBER}
          docker push rachit22/logicapp:latest-${BUILD_NUMBER}
          cd ../sa-webapp/
          docker build -t webapp .
          docker tag webapp rachit22/webapp:latest-${BUILD_NUMBER}
          docker push rachit22/webapp:latest-${BUILD_NUMBER}
          '''
        }
    }
  }
}
