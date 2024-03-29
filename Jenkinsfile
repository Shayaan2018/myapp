#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }

    stage('check java') {
        sh "java -version"
    }
    
    stage('clean') {	
        sh "chmod +x mvnw"	
        sh "./mvnw -ntp clean"	
    }	

    stage('install tools') {	
        sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:install-node-and-npm -DnodeVersion=v10.16.3 -DnpmVersion=6.11.3"	
    }	

    stage('npm install') {	
        sh "./mvnw -ntp com.github.eirslett:frontend-maven-plugin:npm"	
    }	

    stage('packaging') {	
        sh "/usr/bin/mvn -T 4 clean verify -DskipTests"	
        //archiveArtifacts artifacts: '**/target/*.war', fingerprint: true	
    }	

	stage('backend tests') {	
        try {	
            sh "./mvnw -ntp verify"	
        } catch(err) {	
            throw err	
        } finally {	
            junit '**/target/test-results/**/TEST-*.xml'	
        }	
    }	

	stage('quality analysis') {	
            sh "./mvnw -T 6 sonar:sonar -Dmaven.skip.test=true -Dsonar.host.url=http://localhost:9000/ -Dsonar.login=admin -Dsonar.password=adminkey"	
    }

	stage('depoy to tomcat') {
			sh "rm -rf /opt/apache-tomcat-8.5.46/webapps/ROOT.war"
		    sh "cp target/*.war /opt/apache-tomcat-8.5.46/webapps/ROOT.war"
	        sh "/bin/bash /opt/apache-tomcat-8.5.46/bin/startup.sh"
			
	}
	
}
