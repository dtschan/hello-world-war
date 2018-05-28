import com.jenkinsci.plugins.badge.action.BadgeAction

@NonCPS
def buildPromoted() {
    def build = Jenkins.instance.getItemByFullName(built_name).getBuild(built_number)
//    addBadge icon: '/userContent/16x16/star-gold.png', text: "Deployed ${built_name} #${built_number} to ${target_env}", link: "/${build.getUrl()}"
//    createSummary icon: '/userContent/48x48/star-gold.png', text: "Deployed <a href=\"/${build.getJob().getUrl()}\">${built_name}</a> <a href=\"/${build.getUrl()}\">#${build_number}</a> to ${target_env}"
    build.addAction(BadgeAction.createBadge('star-gold.png', "deployed to ${target_env}", "/${currentBuild.rawBuild.getUrl()}"))
    def summary = new BadgeSummaryAction('/userContent/48x48/star-gold.png')
    summary.appendText("Deployed to ${target_env} by <a href=\"/${currentBuild.rawBuild.getJob().getUrl()}\">${env.BUILD_NAME}</a> <a href=\"/${currentBuild.rawBuild.getUrl()}\">#${env.BUILD_NUMBER}</a>")
    build.keepLog(true)
}

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
                buildPromoted()
               //copyArtifacts(projectName: 'hello-world-war', selector: specific("${built.number}"));
            }
        }
    }
}
