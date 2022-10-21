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
                    
                    bat 'mvn --version'
                    println "Maven building"
                    bat 'mvn clean package'
                    println "build Succesfully"
                }
            }
        }
        
         stage('SonarQube analysis') {
            
            //def scannerHome = tool 'sonarqube-4.7';
            
            steps {
                withSonarQubeEnv('sonarqube-server') { 
                    bat "mvn sonar:sonar"
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
