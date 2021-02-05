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
//                        sh './gradlew -Dgob.evn=firefoxHeadless iT'
                        sh './gradlew iT'
                        sh './gradlew codenarcTest'
                    }
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
        stage('Analysis') {

            steps {
                withGradle {
			sh './gradlew checkstyleMain'                    
                }
                recordIssues enabledForFailure: true, tool: checkStyle()              
            }

            def checkstyle = scanForIssues tool: checkStyle(pattern: '**/target/checkstyle-result.xml'
            publishIssues issues: [checkstyle]                    

        }
    }
}
