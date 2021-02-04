#!/usr/bin/env groovy
pipeline {
    agent any
    options {
        ansiColor('xterm')
    }
    //tools {
        //jdk 'OpenJDK-15.0.2'
    //}
    stages {
        stage('Build') {
            steps {
                withGradle{
                    sh './gradlew assemble'
                    
                } 
            }
            /*post {
                success {
                    archiveArtifacts 'build/libs/*.jar'
                }
            }*/
        }
        stage('Test') {
            steps {
                withGradle{
                    configFileProvider(
                        [configFile(fileId: 'hello-grails-gradle.properties')]) {
                        sh './gradlew clean test'
                        sh './gradlew iT'
                        sh './gradlew codenarcTest'
                    }
                    
                }  
            }
            post {
                always {
                    junit 'build/test-results/**/TEST-*.xml'
                    //archiveArtifacts 'build/reports/*'
                    echo 'Publish Codenarc report'
                    publishHTML(
                        target: [
                            allowMissing         : false,
                            alwaysLinkToLastBuild: false,
                            keepAll              : true,
                            reportDir            : 'build/reports/codenarc',
                            reportFiles          : '*.html',
                            reportName           : "Codenarc Report"
                        ]
                    )
                }
            }
        }
    }
}
