pipeline {
    agent any
    
    environment {
        TOMCAT_URL = 'http://127.0.0.1:8080'   // The Tomcat server URL
        WAR_FILE = '/home/gowtham/actions-runner/_work/SLCICDProject/SLCICDProject/target/myproject-1.0.war' // Path to the WAR file
        TOMCAT_USER = 'admin'           // Tomcat admin username (if required for authentication)
        TOMCAT_PASSWORD = 'admin'       // Tomcat admin password (use Jenkins credentials store for security)
        TOMCAT_MANAGER_PATH = '/manager/text'    // Path to the Tomcat Manager app
        TOMCAT_DEPLOY_URL = '/manager/text/deploy' // Deploy URL endpoint
    }
    
    stages {
        stage('Deploy WAR to Tomcat') {
            steps {
                script {
                    echo "Deploying WAR file to Tomcat"

                    // Use curl or wget to deploy the WAR to Tomcat via HTTP
                    sh '''
                        # Use curl to deploy the WAR file to Tomcat
                        curl -u $TOMCAT_USER:$TOMCAT_PASSWORD -T $WAR_FILE "$TOMCAT_URL$TOMCAT_MANAGER_PATH$TOMCAT_DEPLOY_URL?path=/myproject&update=true"
                    '''
                }
            }
        }
    }
    
    post {
        success {
            echo 'WAR file successfully deployed to Tomcat'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}

