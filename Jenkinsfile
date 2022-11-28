pipeline{
  agent {label 'kubeagent'}
  environment{
    Branch="master"
  }
stages{
  stage('Go to repo'){
    steps{
        script{
          echo "take code from github"
          git branch: 'master',
                credentialsId: 'github',
                url: 'https://github.com/aarshsqaureops/k8s-mastery.git'
             echo "Branch copied"
        }
    }
  } 
  stage('Build Image and push to dockerhub'){
    steps{
        script{
          echo "Test code from github"
          sh  ''' 
          echo "dckr_pat_gllAO-xQXrEgBUchziw0wXcxHoY"| docker login --username aarshsquareops --password-stdin
          cd sa-frontend
          docker build -t frontapp .
          docker tag frontapp aarshsquareops/frontapp:latest-${BUILD_NUMBER}
          docker push aarshsquareops/frontapp:latest-${BUILD_NUMBER}
          cd ../sa-logic/
          docker build -t logicapp .
          docker tag logicapp aarshsquareops/logicapp:latest-${BUILD_NUMBER}
          docker push aarshsquareops/logicapp:latest-${BUILD_NUMBER}
          #cd ../sa-webapp/
          #docker build -t webapp .
          #docker tag webapp aarshsquareops/webapp:latest-${BUILD_NUMBER}
          #docker push aarshsquareops/webapp:latest-${BUILD_NUMBER}
          '''
        }
    }
  }
  
  }
  }
