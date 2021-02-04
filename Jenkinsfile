#!/usr/bin/env groovy
pipeline {
    agent any
    options {
        ansiColor('xterm')
    }

    configFileProvider(
        [configFile(fileId: 'hello-grails-gradle.properties', targetLocation: 'gradle.properties')]) {
    }
    stages {
        stage('Test') {
            steps {
                withGradle {
//                    sh './gradlew test'
//                    sh './gradlew -Dgob.evn=firefoxHeadless iT'
                    sh './gradlew iT'
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
