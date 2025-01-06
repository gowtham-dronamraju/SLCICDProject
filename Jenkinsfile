pipeline {
    agent any

    environment {
        WAR_FILE_PATH = '/var/lib/jenkins/workspace/maven_pipeline/target/myproject-1.0.war'
        TOMCAT_HOME = "/opt/tomcat"  // Update this to your Tomcat home directory
        TOMCAT_URL = 'http://localhost:8080' // URL for Tomcat 
        TOMCAT_CREDS = credentials('tomcat_creds')  // Jenkins credentials ID for Tomcat
    }

    tools {
        maven 'MAVEN_HOME'
    }

    stages {
        stage('Git repo Checkout') {
            steps {
                // Checkout the source code from the repository
                git 'https://github.com/gowtham-dronamraju/HelloWorld-WAR.git'
            }
        }

        stage('Maven Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Maven Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build Success') {
            steps {
                echo "Build Successful"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    // Ensure the WAR file exists
                    if (fileExists(WAR_FILE_PATH)) {
                        echo "WAR file found at ${WAR_FILE_PATH}"
                    } else {
                        error "WAR file does not exist at ${WAR_FILE_PATH}"
                    }
                    

                    // Undeploy the existing application if it exists

                    def tomcatAppPath = "/myproject"
                    def tomcatUrl = "${TOMCAT_URL}/manager/text"
                    
                    echo "Undeploying existing application at ${tomcatAppPath}"
                    
                    sh """
                    curl -u ${TOMCAT_CREDS_USR}:${TOMCAT_CREDS_PSW} \
                    --silent --request POST ${tomcatUrl}/undeploy?path=${tomcatAppPath}
                    """


                    // Deploy WAR to Tomcat
                    echo "Deploying WAR file to Tomcat server at ${TOMCAT_URL}"

                    // Use curl to deploy the WAR file
                    sh """
                        curl -u ${TOMCAT_CREDS_USR}:${TOMCAT_CREDS_PSW} \
                        --upload-file ${WAR_FILE_PATH} \
                        ${TOMCAT_URL}/manager/text/deploy?path=/myproject&update=true
                    """
                }
            }
        }
    }

    post {
        success {
            script {
                echo 'Deployment was successful!'
            }
        }
        failure {
            script {
                echo 'Deployment failed.'
            }
        }
        always {
            script {
                echo 'Pipeline execution finished.'
            }
        }
    }
}

