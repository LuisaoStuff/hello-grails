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
//                        sh './gradlew iT'
//                        sh './gradlew codenarcTest'
                        sh './gradlew checkstyleMain'                    
                    }
                }
            }
            post {
                always {
                    recordIssues enabledForFailure: true, tool: checkStyle()              

//                    junit 'build/test-results/**/TEST-*.xml'
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
/*
        stage('Analysis') {

            withGradle {
                sh './gradlew checkstyleMain'                    
            }
*/
//            def checkstyle = scanForIssues tool: checkStyle(pattern: '**/target/checkstyle-result.xml'
/*            publishIssues issues: [checkstyle]                    

        }
*/
    }
}
