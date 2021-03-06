#!groovy

@Library('github.com/tpbtools/jenkins-pipeline-library@v4.0.0') _

// Initialize global config
cfg = jplConfig('devcontrol', 'bash', '', [email: env.CI_NOTIFY_EMAIL_TARGETS])

pipeline {
    agent { label 'docker' }

    stages {
        stage ('Initialize') {
            steps  {
                jplStart(cfg)
            }
        }
        stage ('Test') {
            steps  {
                sh 'bin/test.sh'
            }
        }
        stage ('Make release') {
            when { branch 'release/new' }
            steps {
                jplMakeRelease(cfg, true)
            }
        }
    }

    post {
        always {
            jplPostBuild(cfg)
        }
        cleanup {
            deleteDir()
        }
    }

    options {
        timestamps()
        ansiColor('xterm')
        buildDiscarder(logRotator(artifactNumToKeepStr: '20',artifactDaysToKeepStr: '30'))
        disableConcurrentBuilds()
        timeout(time: 10, unit: 'MINUTES')
    }
}
