pipeline {
    agent any
    tools {
        maven "MAVEN3.9"
        jdk "JDK17"
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }

            post {
                success {
                    echo 'Now archiving'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('unit test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('checkstyle analysis') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('sonarqube analysis') {
            environment {
                scannerHome = tool 'sonar'
            }
            steps {
                withSonarQubeEnv('sonarscanner') {
                sh """${scannerHome}/bin/sonar-scanner \
                   -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/main/java \
                   -Dsonar.java.binaries=target/classes \
                   -Dsonar.java.test.binaries=target/test-classes \
                   -Dsonar.junit.reportsPath=target/surefire-reports \
                   -Dsonar.coverage.jacoco.xmlReportPaths=target/site/jacoco/jacoco.xml \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml"""
                }

            }
        }


    }
}