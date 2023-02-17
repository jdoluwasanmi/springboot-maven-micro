do not forget to add maven and its version in global tool configuration.


pipeline {
        agent any
        tools{
            maven '3.8.5'
        }
        stages {
            stage("Checkout the project") {
               steps {
                   git branch: 'master', url: 'https://github.com/jdoluwasanmi/springboot-maven-micro.git'
               }
            }
        stage("build the package") {
            steps {
                sh 'mvn clean package'
            }
        }    
        stage("sonar quality check") {
        steps {
            script {
             withSonarQubeEnv(installationName: 'Sonarqube', credentialsId: 'sonarjenk') {
             sh 'mvn sonar:sonar'
            }
        }
      }
    }
    }
}
