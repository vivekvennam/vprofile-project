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
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
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
     stage('Sonar Analysis') {

            environment {
                scannerHome = tool "${SONARSCANNER}"
            }

            steps {

                withSonarQubeEnv("${SONARSERVER}") {

                    sh '''
                    ${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=vprofile \
                    -Dsonar.projectName=vprofile \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=src/ \
                    -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                    -Dsonar.junit.reportsPath=target/surefire-reports/ \
                    -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml
                    '''
                }
            }
        }
        stage("Quality Gate") {
    steps {
        timeout(time: 1, unit: 'HOURS') {

            waitForQualityGate abortPipeline: true
        }
    }
}
    }
    

}