pipeline {
  agent any
  tools {
	  maven 'M2_HOME'    
  }
  

  stages {
       
    stage ('Artifact construction') {
            steps {
                sh 'echo "Artifact construction is processing ...."'
                sh 'mvn -DskipTests package'
            }
            
        }

     
    stage("SonarQube ") {
            steps {
              withSonarQubeEnv('SonarQube') {
                sh 'mvn clean -DskipTests package sonar:sonar'
              }
            }
          }
    stage("NEXUS") {
			steps {
						sh 'mvn clean deploy -DskipTests'
      }
    }
	  stage('Docker build image') {
      steps {
         sh 'echo "Docker build image is processing ...."'
        sh 'docker build -t noamenn/achat:1.1 .'

      }
    }
     stage('Docker login') {
      steps {
         sh 'echo "Docker login is processing ...."'
	      sh 'docker login --username noamenn --password ${DOCKERHUBPASSWD}'

      }
    }
    stage('Docker push') {
      steps {
         sh 'echo "Docker push is processing ...."'
        sh 'docker push noamenn/achat:1.1'

      }
    }
	stage('Running Back') {
      		steps {
         		sh 'docker-compose up -d'
      }
    }
  }
}
