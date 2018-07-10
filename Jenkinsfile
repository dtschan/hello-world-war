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
                /*withMaven(maven: 'M3') {
                    sh "mvn -B -V -U -e clean verify -DskipTests"
                }*/
                echo 'Building'                
            }
        }
        stage('Test') {
            steps {
                withMaven(maven: 'M3') {
                    sh "mvn -B -V -U -e deploy -Dsurefire.useFile=false"
                }
                echo 'Running Tests'                
                addHtmlBadge html:"<a href=\"/view/build-promotion/job/hello-world-war-deploy/job/master/parambuild?delay=0sec&built_name=${JOB_NAME}&built_number=${BUILD_NUMBER}\">Deploy</a> "
            }
        }
    }
}
