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
                    recordIssues (
                        enabledForFailure: true,
                        tool: checkStyle(pattern: 'build/reports/checkstyle/*.xml')
                    )
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
    }
}
