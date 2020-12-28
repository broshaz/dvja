pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
    stage ('Check-The-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        //sh 'docker run gesellix/trufflehog --regex --entropy=False https://github.com/broshaz/webLemah.git --json > trufflehog'
        sh 'docker run gesellix/trufflehog --regex --entropy=True file:./src/* --json > keputusanTrufflehog'
        sh 'cat trufflehog'
       // sh 'git clone https://github.com/dxa4481/truffleHog.git'
        
      }
    }
    stage ('Software Composition Analysis') {
      steps {
         sh 'rm -r dependency-check* || true' 
         sh 'wget https://github.com/jeremylong/DependencyCheck/releases/download/v6.0.3/dependency-check-6.0.3-release.zip'
         sh 'unzip dependency-check-6.0.3-release.zip'
         sh './dependency-check/bin/dependency-check.sh --scan ./src/* --enableRetired -f "ALL" '
               }
    }
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    stage ('Deploy-To-Tomcat') {
            steps {
           sshagent(['tomcat']) {
                sh 'scp -o StrictHostKeyChecking=no target/dvja-1.0-SNAPSHOT.war ubuntu@35.247.137.87:/home/ubuntu/prod/apache-tomcat-8.5.61/webapps/dvja.war'
              }      
           }      
    }  
  }
}
