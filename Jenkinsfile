#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }

    stage('check java') {
        sh "java -version"
    }

	stage('depoy to tomcat') {
	        sh "/opt/apache-tomcat-8.5.46/bin/shutdown.sh"
			sh "rm -rf /opt/apache-tomcat-8.5.46/webapps/ROOT*"
			sh "rm -rf /opt/apache-tomcat-8.5.46/temp/*"
            sh "rm -rf /opt/apache-tomcat-8.5.46/logs/*"
		    sh "cp target/*.war /opt/apache-tomcat-8.5.46/webapps/ROOT.war"
			sh "sleep 3s"
	        sh "/bin/bash /opt/apache-tomcat-8.5.46/bin/startup.sh"
			
	}
	
}
