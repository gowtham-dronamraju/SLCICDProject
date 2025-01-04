pipeline {
    agent any

    environment {
        TOMCAT_HOME = "/opt/tomcat"  // Update this to your Tomcat home directory
        TOMCAT_USER = "admin"  // Tomcat user with manager privileges
        TOMCAT_PASS = "admin_password"  // Tomcat password for manager
        TOMCAT_URL = "http://localhost:8080/manager/text"  // URL for Tomcat manager
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
                    // Path to your generated WAR file
                    def warFile = '/var/lib/jenkins/workspace/war_tomcat_freestyle/target/myproject-1.war'

                    // Command to deploy to Tomcat via the Tomcat Manager
                    sh """
                        curl -u ${TOMCAT_USER}:${TOMCAT_PASS} \
                        --upload-file ${warFile} \
                        ${TOMCAT_URL}/deploy?path=/myproject&update=true
                    """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment was successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }    
