
Sonarqube Integration with Jenkins123
----------------------------------------------------
Prerequisites:

(1) A Jenkins Server
(2) A Sonarqube Server

Integration Steps:

On Sonarqube server:

(1)generate a token 

Go to "My Account > Security" and generate a token on that page

Create a project and generate a token:
project-name:<project-name>
sonarqube-test: <token>

Manually testing using cmd: 
    mvn clean install  sonar:sonar -Dsonar.login=<token> -Dsonar.projectName=<project-name>

On Jenkins Server:

(1) Install Sonarqube Plugin

  (2) Configure Sonarqube Credentials with (Secret Text) Credentials type

(3) Add Sonarqube to jenkins "configure system"
	Manage Jenkins ->Configure Jenkins -> SonarQube servers and fill the details:
	name: sonarqube
	Server Url: http://localhost:9000/
	Server Authentication Token : Created Secret
  
(4) Install Sonarscanner
	Manage Jenkins -> Global Configuration ->SonarQube Scanner ->install

(5) Add Soanrqube stage to pipeline and run pipeline
	
	  stage('SonarQube analysis') {
        //def scannerHome = tool 'SonarScanner 4.0';
        steps{
          withSonarQubeEnv('sonarqube-8.9') { 
          // If you have configured more than one global server connection, you can specify its name
         //sh "${scannerHome}/bin/sonar-scanner"
         bat "mvn sonar:sonar"
        }
      }
    }
       
