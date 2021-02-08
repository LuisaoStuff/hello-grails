#!/usr/bin/env groovy
pipeline {
    agent any
    options {
        ansiColor('xterm')
    }

    stages {
        stage('Test') {
            steps {
                configFileProvider([configFile(fileId: 'hello-grails-gradle.properties', targetLocation: 'gradle.properties')]) {
                    withGradle {
                        sh './gradlew clean check'
//                        sh './gradlew iT'
//                        sh './gradlew codenarcTest'
//                        sh './gradlew checkstyleTest'
                    }
                }
            }
            post {
                always {                    
                    junit 'build/test-results/**/TEST-*.xml'
                    recordIssues enabledForFailure: true, tool: checkStyle(pattern: 'build/reports/checkstyle/*.xml')
                    recordIssues enabledForFailure: true, tool: spotBugs(pattern: 'build/reports/spotbugs/*.xml')
                    recordIssues enabledForFailure: true, tool: pmdParser(pattern: 'build/reports/pmd/*.xml')
                    publishHTML (target : [allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'build/reports/codenarc',
                        reportFiles: '*.html',
                        reportName: 'Reportes',
                        ])
                        
                }
            }
        }
        stage('SonarQube') {
            steps {
                withSonarQubeEnv(credentialsId: 'a821f47c-66dd-4888-859c-90d41bcf26b6', installationName: 'Sonarqube') {
                    sh './gradlew sonarqube'
                }
            }
        }
        stage('Build') {
            steps {
                withGradle {
                    sh './gradlew assemble'
                }
                post {
                    success {
                        junit 'build/jacoco/*.xml'
                        step( [ $class: 'JacocoPublisher' ] )
                    }
                }
            }
        }
    }
}
