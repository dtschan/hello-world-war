import com.jenkinsci.plugins.badge.action.BadgeAction
import com.jenkinsci.plugins.badge.action.BadgeSummaryAction

@NonCPS
def buildPromoted() {
    def build = Jenkins.instance.getItemByFullName(built_name).getBuild(built_number)
    def deploy = currentBuild.rawBuild
//    addBadge icon: '/userContent/16x16/star-gold.png', text: "Deployed ${built_name} #${built_number} to ${target_env}", link: "/${build.getUrl()}"
//    createSummary icon: '/userContent/48x48/star-gold.png', text: "Deployed <a href=\"/${build.getParent().getUrl()}\">${built_name}</a> <a href=\"/${build.getUrl()}\">#${build_number}</a> to ${target_env}"
    deploy.addAction(BadgeAction.createBadge('/userContent/16x16/star-gold-e.png', "Deployed ${built_name} #${built_number} to ${target_env}", "/${build.getUrl()}"))
    def summary = new BadgeSummaryAction('/userContent/48x48/star-gold-e.png')
    summary.appendText("Deployed <a href=\"/${build.getParent().getUrl()}\">${built_name}</a> <a href=\"/${build.getUrl()}\">#${built_number}</a> to ${target_env}")
    deploy.addAction(summary)
    build.addAction(BadgeAction.createBadge('/userContent/16x16/star-gold.png', "Deployed to ${target_env} by ${env.JOB_NAME} #${env.BUILD_NUMBER}", "/${currentBuild.rawBuild.getUrl()}")) 
    summary = new BadgeSummaryAction('/userContent/48x48/star-gold.png')
    summary.appendText("Deployed to ${target_env} by <a href=\"/${currentBuild.rawBuild.getParent().getUrl()}\">${env.JOB_NAME}</a> <a href=\"/${currentBuild.rawBuild.getUrl()}\">#${env.BUILD_NUMBER}</a>")
    build.addAction(summary)
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
            when {
                expression {                
                    return built_name && built_number
                }
            }
            steps {
                    echo "Deploying ${built_name} build #${built_number} to ${target_env}"
                    buildPromoted()               
            }
        }
    }
}
