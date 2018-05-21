pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timestamps()
        ansiColor('xterm')
    }
    /*triggers {
        pollSCM('H/5 * * * *')
        snapshotDependencies()
    }*/
    stages {
        stage('Build') {
            steps {
                withMaven(maven: 'M3' {
                    sh "mvn -B -V -U -e clean verify"
                }
            }
        }
        stage('Test') {
            steps {
                withMaven(maven: 'M3' {
                    sh "mvn -B -V -U -e verify -Dsurefire.useFile=false"
                }
            }
        }
    }
}
