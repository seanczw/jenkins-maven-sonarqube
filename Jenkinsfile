pipeline {
    agent any 
    
    stages {
        
        stage("clone the git") {
            steps {
                echo "Cloning the git"
                git branch: 'master', url: 'https://github.com/seanczw/jenkins-maven-sonarqube'
                echo "Git cloned"
            }
        }
        
        stage("Building Using Maven"){
            steps{
                
                script {
                    sh 'mvn --version'
                    println "Maven building"
                    sh 'mvn clean package'
                    println "build Succesfully"
                   }
      
                }
            }
        }
        
         stage('SonarQube analysis') {
            
            //def scannerHome = tool 'sonarqube-4.7';
            
            steps {
                withSonarQubeEnv('sonarqube-server') { 
                    sh "mvn sonar:sonar"
                }
            }
        }
        
         stage("Quality gate") {
            
            steps {
                
                script {
                    println "Waiting for the QualityGate Check"
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
            }
        }
       
    }
}
