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
   /*
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
    }*/
 
    stage ('Software Composition Analysis') {
      steps {
         //sh 'rm -r dependency-check* || true' 
         //sh 'wget https://github.com/jeremylong/DependencyCheck/releases/download/v6.2.1/dependency-check-6.2.1-release.zip'
         //sh 'unzip dependency-check-6.2.1-release.zip'
         //sh './dependency-check/bin/dependency-check.sh --scan ./src/* --enableRetired -f "ALL" '
      //   sh 'rm owasp* || true'
       //  sh 'wget "https://raw.githubusercontent.com/broshaz/dvja/master/owasp-dependency-check.sh" '
       //  sh 'chmod +x owasp-dependency-check.sh'
       //  sh 'bash owasp-dependency-check.sh'
       //  sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
      //dependencyCheck additionalArguments: 'scan="./src/*" --enableRetired -format "ALL"', odcInstallation: 'OWASP Dependency Check Scanner'
      //dependencyCheck additionalArguments: 'scan="./src/*" --enableRetired -format "ALL"', odcInstallation: 'ODC-6.1.6-Extractzip'
      //dependencyCheck additionalArguments: 'scan="./src/*" --enableRetired -format "ALL"', odcInstallation: 'ODC-bintray-6.2.1'  
      //dependencyCheckPublisher pattern: 'dependency-check-report.xml'
      //mail bcc: '', body: 'Please check your report ODC here http://34.72.61.98:8080/job/webapp-cicd-dvja/65/execution/node/3/ws/dependency-check-report.xml', cc: '', from: '', replyTo: '', subject: 'Your Findings Report', to: 'shazil.imri@gmail.com'
      //dependencyTrackPublisher artifact: 'bom.xml', autoCreateProjects: false, dependencyTrackApiKey: 'API_KEY', dependencyTrackFrontendUrl: 'http://35.232.237.165:8080', dependencyTrackUrl: 'http://35.232.237.165:8080', projectId: 'a4024f95-2dbc-4c7a-9206-2249c1b22e29', synchronous: true
      withCredentials([string(credentialsId: '506ed685-4e2b-4d31-a44f-8ba8e67b6341', variable: 'API_KEY')]) {
                    dependencyTrackPublisher artifact: 'target/bom.xml', projectName: 'my-project', projectVersion: 'my-version', synchronous: true, dependencyTrackApiKey: API_KEY  
        
        emailext body: '''$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS:
            Check console output at $BUILD_URL to view the results.''', subject: '$PROJECT_NAME - Build # $BUILD_NUMBER - $BUILD_STATUS', to: 'shazil.imri@gmail.com' 

      }
    }
 
    stage ('Build') {
      steps {
      sh 'mvn clean package'
      
       }
    }
    stage ('Deploy-To-Tomcat') {
            steps {
         //     sh 'cp target/dvja-1.0-SNAPSHOT.war /var/lib/tomcat9/webapps/dvja.war'
           sshagent (['tomcat']) {
                // yg asal sh 'scp -o StrictHostKeyChecking=no target/dvja-1.0-SNAPSHOT.war ubuntu@35.247.137.87:/home/ubuntu/prod/apache-tomcat-8.5.61/webapps/dvja.war'
                //sh 'scp -o StrictHostKeyChecking=no target/*.war penggunabitorb4@35.224.19.78:/home/penggunabitorb4/prod/apache-tomcat-8.5.65/webapps/dvja.war'
                sh 'scp -o StrictHostKeyChecking=no target/*.war penggunabitorb4@instance-devsecops-tomcat:/home/penggunabitorb4/prod/apache-tomcat-8.5.65/webapps/ROOT.war'
              }  
           cleanWs(patterns: [[pattern: 'dependency-check-report.json', type: 'INCLUDE']])
           }      
    }
     
  }
}
