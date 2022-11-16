pipeline {
  agent any
  tools {
     jdk 'JAVA_HOME'
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
                sh 'mvn sonar:sonar -Dsonar.projectKey=tp -Dsonar.host.url=http://192.168.56.2:9000 -Dsonar.login=ac603abcffe91de301ee261de17523103d68618a'
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
        sh 'docker build -t maryemcherif/achat:1.0 .'

      }
    }
     stage('Docker login') {
      steps {
         sh 'echo "Docker login is processing ...."'
	      sh 'docker login --username maryemcherif --password ${DOCKERHUBPASSWD}'

      }
    }
    stage('Docker push') {
      steps {
         sh 'echo "Docker push is processing ...."'
        sh 'docker push maryemcherif/achat:1.0'

      }
    }
	stage('Running Back') {
      		steps {
         		sh 'docker-compose up -d'
      }
    }
  }
}
