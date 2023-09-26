pipeline {
    agent any
   // {
   //   label 'linux'
   //  }
     tools {
      maven '3.8.4'
     }
    stages {
        stage('Checkout') {
            steps {
                // Check out your Git repository here
                git url: 'https://github.com/Hvasan/time-tracker.git', branch: 'master'
            }
        }

        stage('Build and Deploy') {
            steps {
                script {
                    // Define the path to your pom.xml file
                    def pomFile = 'time-tracker/pom.xml'
                    // Set the Maven version
                   // def mavenHome = tool name: 'Maven 3.8.4', type: 'hudson.tasks.Maven$MavenInstallation'
                    sh 'mvn --version'
                    // Clean and deploy with Maven
                   // def mvnCmd = "${mavenHome}/bin/mvn"
                   // def deployCmd = "${mvnCmd} clean deploy -f ${pomFile}"
                    sh 'mvn -f ${pomFile}, clean deploy' 

                    // Execute the Maven command
                    sh script: deployCmd, returnStatus: true

                    if (currentBuild.resultIsWorseOrEqualTo('FAILURE')) {
                        error("Maven build failed. Aborting.")
                    }

                    // Push artifacts to Anypoint MuleSoft Exchange
                    def exchangeCmd = "${mavenHome}/bin/mvn some-mulesoft-exchange-plugin-command -f ${pomFile}"
                    sh script: exchangeCmd, returnStatus: true

                    if (currentBuild.resultIsWorseOrEqualTo('FAILURE')) {
                        error("Pushing artifacts to MuleSoft Exchange failed. Aborting.")
                    }

                    // Deploy the application (add your deployment commands here)
                    def deployAppCmd = "some-deployment-command"
                    sh script: deployAppCmd, returnStatus: true

                    if (currentBuild.resultIsWorseOrEqualTo('FAILURE')) {
                        error("Deployment failed. Aborting.")
                    }
                }
            }
        }
    }
}
