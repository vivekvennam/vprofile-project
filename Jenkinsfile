pipeline {
    agent any
    tools {
        maven "maven3.9"
        jdk "Java-21"
    }
    
    environment {
        SNAP_REPO = 'vprofile-snapshot'
		NEXUS_USER = 'admin'
		NEXUS_PASS = 'admin123'
		RELEASE_REPO = 'vprofile-release'
		CENTRAL_REPO = 'vpro-maven-central'
		NEXUSIP = '172.31.24.145'
		NEXUSPORT = '8081'
		NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
    }

    stages {
        stage('Build'){
            steps {
                sh 'mvn -s settings.xml -DskipTests install'
            }

        
        post {
            always {
            echo 'Build stage completed.'
            archiveArtifacts artifacts: '**/*.war', fingerprint: true
            echo ' done bro finally'
            }
        }
        
        }
    
    stage('Test') {
        steps {
            sh 'mvn test'
        }
    }
    stage('checkstyle') {
        steps {
            sh 'mvn checkstyle:checkstyle'
        }
    }
    }

}