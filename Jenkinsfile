pipeline {
    agent any

//	tools {
//		maven 'maven3.6'
//	}
//	environment {
//		M2_INSTALL = "/usr/bin/mvn"
//	}

    stages {
		stage('Clone-Repo') {
			steps {
				checkout scm
			}
		}
	
		stage('Build') {
			steps {
				sh 'mvn install -Dmaven.test.skip=true'
			}
		}
		
		stage('Unit Tests') {
			steps {
				sh 'mvn compiler:testCompile'
				sh 'mvn surefire:test'
				junit 'target/**/*.xml'
			}
		}

	
		stage('Deployment') {
			steps {
				sh 'sshpass -p "svbs" scp /root/.jenkins/workspace/SVBSKart-Pipline/target/SVBSKart.war svbs@172.17.0.3:/home/svbs/apache-tomcat-8.5.70/webapps/'
				sh 'sshpass -p "svbs" ssh svbs@172.17.0.3 "sh /home/svbs/apache-tomcat-8.5.70/bin/shutdown.sh"'

				sh 'sshpass -p "svbs" ssh svbs@172.17.0.3 "sh /home/svbs/apache-tomcat-8.5.70/bin/startup.sh"'
			}

		}
    }
}
