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
        sh 'docker pull gesellix/trufflehog'
        //sh 'docker pull dxa4481/trufflehog'
        sh 'docker run gesellix/trufflehog --json https://github.com/broshaz/dvja.git > keputusanTrufflehogDVJA.json'
        //sh 'docker run gesellix/trufflehog --json file:///* > keputusanTrufflehogDVJAgesellix'
        //sh 'docker run dxa4481/trufflehog https://github.com/broshaz/dvja.git --json > keputusanTrufflehogDVJAdxa4481'
        //sh 'docker run gesellix/trufflehog --regex --entropy=False  https://github.com/broshaz/dvja.git > keputusanTrufflehogDVJAjson.json'
        //sh '''' set +e '''
        sh 'cat keputusanTrufflehogDVJA.json'
        //sh 'rm keputusanTrufflehog'
        //sh 'rm keputusanTrufflehog.json'
            
      }
    }
 /*
    stage ('Software Composition Analysis') {
      steps {
      //   sh 'rm -r dependency-check* || true' 
      //   sh 'wget https://github.com/jeremylong/DependencyCheck/releases/download/v6.0.3/dependency-check-6.0.3-release.zip'
      //   sh 'unzip dependency-check-6.0.3-release.zip'
      //   sh './dependency-check/bin/dependency-check.sh --scan ./src/* --enableRetired -f "ALL" '
         sh 'rm owasp* || true'
         sh 'wget "https://raw.githubusercontent.com/broshaz/dvja/master/owasp-dependency-check.sh" '
         sh 'chmod +x owasp-dependency-check.sh'
         sh 'bash owasp-dependency-check.sh'
         sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
      }
    }
    */
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    stage ('Deploy-To-Tomcat') {
            steps {
         //     sh 'cp target/dvja-1.0-SNAPSHOT.war /var/lib/tomcat9/webapps/dvja.war'
           sshagent(['tomcat']) {
                // yg asal sh 'scp -o StrictHostKeyChecking=no target/dvja-1.0-SNAPSHOT.war ubuntu@35.247.137.87:/home/ubuntu/prod/apache-tomcat-8.5.61/webapps/dvja.war'
                'scp -o StrictHostKeyChecking=no target/dvja-1.0-SNAPSHOT.war penggunabitorb6@34.66.14.171:/home/penggunabitorb6/apache-tomcat-9.0.41/webappsdvja.war'
              }      
           }      
    }  
  }
}
