#!/usr/bin/env groovy

def phpVersions = ['56', '70', '71']

def getRunners(debug = false) {
    def runners = [:]

    for (String phpVersion : phpVersions) {
        def v = version
        runners["${v}"] = {
            sh "./docker.sh build ${v} ${debug}"
        }
    }

    return runners
}

node('docker') {
    ansiColor('xterm') {
        stage('Checkout') {
            checkout scm
        }

        stage('Build Images') {
            sh './docker.sh images'
        }

        stage('Build') {
            parallel getRunners(false)
        }

        stage('Build with xDebug') {
            parallel getRunners(true)
        }
    }
}
