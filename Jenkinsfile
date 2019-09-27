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
	
	stage('stop tomcat') {
	        deploy adapters: [tomcat8(credentialsId: '3fe2f204-8ee6-44a2-8f3e-5daa27f8c30c', path: '', url: 'http://localhost:8080')], contextPath: 'ROOT', war: '**/target/*.war'
    }
	
	
}
