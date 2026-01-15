post {
    success {
        emailext(
            subject: "SUCCESS: Jenkins Build #${BUILD_NUMBER}",
            body: """\
Jenkins Job: ${JOB_NAME}
Build Number: ${BUILD_NUMBER}
Status: SUCCESS
""",
            to: "adewalekomolafe@gmail.com"
        )
    }
    failure {
        emailext(
            subject: "FAILED: Jenkins Build #${BUILD_NUMBER}",
            body: """\
Jenkins Job: ${JOB_NAME}
Build Number: ${BUILD_NUMBER}
Status: FAILURE
Check Jenkins console output.
""",
            to: "adewalekomolafe@gmail.com"
        )
    }
}

