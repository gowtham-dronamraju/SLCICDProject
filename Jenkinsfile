pipeline {
    agent any
    
    environment {
        TOMCAT_SERVER = 'http://127.0.0.1:8080'
        TOMCAT_DEPLOY_DIR = '/opt/tomcat/webapps'  // Update with actual Tomcat webapps directory
        APP_NAME = 'HelloWorld-WAR'  // Update with your application name
        TOMCAT_USER = 'admin'  // Replace with your Tomcat username
        TOMCAT_PASS = 'admin'  // Replace with your Tomcat password
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository from GitHub
                git 'https://github.com/gowtham-dronamraju/HelloWorld-WAR.git'
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    // Assuming WAR file is downloaded from GitHub Actions
                    def warFile = params.warFile  // WAR file path passed from GitHub Actions

                    // Copy the WAR file to the Tomcat webapps directory
                    sh "cp ${warFile} ${TOMCAT_DEPLOY_DIR}"

                    // Restart Tomcat to deploy the WAR
                    sh """
                    curl -u ${TOMCAT_USER}:${TOMCAT_PASS} ${TOMCAT_SERVER}/manager/text/reload?path=/${APP_NAME}
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}

