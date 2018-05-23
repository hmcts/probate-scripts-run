#!groovy

@Library('Reform')
import uk.gov.hmcts.Ansible
import uk.gov.hmcts.Artifactory
import uk.gov.hmcts.Packager
import uk.gov.hmcts.RPMTagger

def artifactory = new Artifactory(this)
def packager = new Packager(this, 'probate')
def ansible = new Ansible(this, 'probate')
def artifactorySourceRepo = "probate-local"
def rpmTagger

properties([
        [$class: 'GithubProjectProperty', displayName: 'Run Shell Command on Web and App servers', projectUrlStr: 'https://github.com/hmcts/probate-scripts-run.git'],
        parameters([
                string(description: 'environment to  run shell command (dev,test,demo)', defaultValue: 'dev', name: 'deploy_enviroinment'),
                string(description: 'RUN your Web 01/02 Server Script', defaultValue: " ", name: 'runWebShellScript'),
                string(description: 'RUN your App 01/02 Server Script', defaultValue: " ", name: 'runAppShellScript'),
                string(description: 'Reboot Servers (if "no" will not reboot else reboot)', defaultValue: 'no', name: 'reboot')
        ])
])


node {

    try {

        deploy_enviroinment =  (deploy_enviroinment in ['dev', 'test', 'demo']) 
                                ? deploy_enviroinment : error ("$deploy_enviroinment is not one of reforms deployment environments")
        def ansible_branch='master' 
        def azure_tags=''
        def ansible_tags='all'
        def verbose=false;
        stage('Checkout') {
            deleteDir()
            checkout scm
        }

        if(runWebShellScript != " ") {
            stage('Run Web01&02 Shell Script') {
                return ansible.run(runWebShellScript, deploy_enviroinment, 'runShellCommand_fe.yml', ansible_branch, azure_tags, ansible_tags, verbose)
            }
        }

        if(runAppShellScript != " ") {
            stage('Run App01&02 Shell Script') {
                return ansible.run(runAppShellScript, deploy_enviroinment, 'runShellCommand_be.yml', ansible_branch, azure_tags, ansible_tags, verbose)
            }
        }

        stage('show Installed RPM') {
            return ansible.run("rpm -qa probate*", deploy_enviroinment, 'runShellcommand.yml', ansible_branch, azure_tags, ansible_tags, verbose)
        }
            
        if(reboot) {
            
            if(reboot != "no") {
                stage('Reboot servers'){
                    return ansible.run("reboot", deploy_enviroinment, 'reboot_all.yml', ansible_branch, azure_tags, ansible_tags, verbose)
                }
            }
        }
        deleteDir()
    } catch (Throwable err) {
        deleteDir()
        slackSend(
                channel: '#probate-jenkins',
                color: 'danger',
                message: "Run Shell Command for env. " + deploy_enviroinment +" FAILED.")
        throw err
    }
}
