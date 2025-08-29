pipeline {
    agent any

    tools {
        maven 'Maven'   // Use the Maven installation you configured in Jenkins
        jdk 'Java'      // Use Java installation in Jenkins
    }

    environment {
        // Nexus credentials stored in Jenkins (ID = nexus-creds)
        NEXUS_CREDENTIALS = credentials('nexus-creds')
        NEXUS_URL = 'http://kishore.fun:8081/repository/maven-releases/'
        GROUP_ID = 'Kishore'
        ARTIFACT_ID = 'my-app'
        VERSION = '1.0.0'
        PACKAGING = 'jar'
        FILE = "target/${ARTIFACT_ID}-${VERSION}.${PACKAGING}"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/reddi-devops/hello-world-war.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Upload to Nexus') {
            steps {
                sh """
                curl -v -u ${NEXUS_CREDENTIALS_USR}:${NEXUS_CREDENTIALS_PSW} \
                --upload-file ${FILE} \
                ${NEXUS_URL}${GROUP_ID//./\/}/${ARTIFACT_ID}/${VERSION}/${ARTIFACT_ID}-${VERSION}.${PACKAGING}
                """
            }
        }
    }
}
