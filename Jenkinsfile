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
                        sh './gradlew clean'
                        sh './gradlew iT'
//                        sh './gradlew codenarcTest'
                        sh './gradlew checkstyle'
                    }
                }
            }
            post {
                always {                    
                    junit 'build/test-results/**/TEST-*.xml'
                    recordIssues enabledForFailure: true, tool: checkStyle()
/*                    publishHTML (target : [allowMissing: false,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'build/reports/codenarc',
                        reportFiles: '*.html',
                        reportName: 'Reportes',
                        ])
*/                        
                }
            }
        }
    }
}
