#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }

    stage('check java') {
        sh "java -version"
    }

    stage('packaging') {
        sh "/usr/bin/mvn -T 4 clean verify -DskipTests"
        //archiveArtifacts artifacts: '**/target/*.war', fingerprint: true
    }
	stage('stop tomcat') {
	        deploy adapters: [tomcat8(credentialsId: '3fe2f204-8ee6-44a2-8f3e-5daa27f8c30c', path: '', url: 'http://localhost:8080')], contextPath: 'ROOT', war: '**/target/*.war'
    }
	
	
}
