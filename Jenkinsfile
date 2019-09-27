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

    
    }
	stage('stop tomcat') {
	        deploy adapters: [tomcat8(credentialsId: '3fe2f204-8ee6-44a2-8f3e-5daa27f8c30c', path: '', url: 'http://localhost:8080')], contextPath: 'ROOT', war: '**/target/*.war'
    }
	
	
}
