import com.jenkinsci.plugins.badge.action.BadgeAction

pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timestamps()
        ansiColor('xterm')
    }
    parameters {
        string(name: 'built_name', description: 'Name of component to deploy')
        string(name: 'built_number', description: 'Number of build to deploy')
        string(name: 'target_env', defaultValue: 'dev', description: 'Environment to deploy to')
    }
    /*triggers {
        pollSCM('H/5 * * * *')
        snapshotDependencies()
    }*/
    stages {
        stage('Deploy') {
            steps {
                echo "Deploying ${built_name} build #${built_number} to ${target_env}"
                script {
                    build = Jenkins.instance.getItemByFullName(built_name).getBuild(built_number)
                    addBadge icon: '/userContent/16x16/star-gold.png', text: "deployed ${built_name} #${built_number} to ${target_env}", link: "/${build.getUrl()}"
                    build.addAction(BadgeAction.createBadge('star-gold.png', "deployed to ${target_env}", "${currentBuild.rawBuild.getUrl()}/console"))
                    build.keepLog()
                    build = null
                }
               //copyArtifacts(projectName: 'hello-world-war', selector: specific("${built.number}"));
            }
        }
    }
}
