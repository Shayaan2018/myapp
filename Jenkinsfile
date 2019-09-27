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

    stage('backend tests') {
        try {
            sh "./mvnw -ntp verify"
        } catch(err) {
            throw err
        } finally {
            junit '**/target/test-results/**/TEST-*.xml'
        }
    }

    stage('packaging') {
        sh "./mvnw -ntp verify -Pprod -DskipTests"
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
    }
    stage('quality analysis') {
            sh "./mvnw -T 6 sonar:sonar -Dmaven.skip.test=true -Dsonar.host.url=http://localhost:9000/ -Dsonar.login=admin -Dsonar.password=adminkey"
    }
	stage('stop tomcat') {
	        sh "/opt/apache-tomcat-8.5.46/bin/shutdown.sh"
			sh "rm -rf /opt/apache-tomcat-8.5.46/webapps/myapp*"
			sh "rm -rf /opt/apache-tomcat-8.5.46/temp/*"
    }
	stage('depoly package') {
		    sh "cp target/*.war /opt/apache-tomcat-8.5.46/webapps/"
	}
	stage(start tomcat) {
	        sh "/opt/apache-tomcat-8.5.46/bin/shutdown.sh"
	}
	
}
