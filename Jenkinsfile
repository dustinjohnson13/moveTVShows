#!groovy

stage('Run') {
    node {

        def build = "${env.JOB_NAME} - #${env.BUILD_NUMBER}".toString()

        currentBuild.result = "SUCCESS"

        try {

            checkout scm

            // # Symlink the database if it doesn't exist
            sh '[ -f db-config.sqlite ] || ln -s /data/archives/video/tv/downloads/db-config.sqlite'
            sh './gradlew run'
            sh './gradlew postProcessing'

        } catch (err) {
            currentBuild.result = "FAILURE"

            emailext body: "${env.JOB_NAME} failed! See ${env.BUILD_URL} for details.", recipientProviders: [[$class: 'DevelopersRecipientProvider']], subject: "$build failed!"

            throw err
        }
    }
}