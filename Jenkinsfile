#!/usr/bin/env groovy
pipeline {
    agent any
    tools {
        jdk 'openjdk-15.0.2'
    }
    options {
        ansiColor('xterm')
    }
    stages {
        stage('Test') {
            steps {
                withGradle {
                    sh './gradlew test'
//                    sh './gradlew -Dgob.evn=firefoxHeadless iT'
                    sh './gradlew codenarcTest'
                }
            }
            post {
                always {
                    junit 'build/test-results/**/TEST-*.xml'
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
