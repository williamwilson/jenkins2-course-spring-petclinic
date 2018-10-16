pipeline {
    agent any
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')

        file(name: "FILE", description: "Choose a file to upload")
    }

    stages {
        stage('1') {
            notify('Started')

            try {
                stage('checkout') {
                    git 'https://github.com/williamwilson/jenkins2-course-spring-petclinic.git'
                }

                stage('build') {
                    sh 'mvn clean package'
                }
            }
            catch (err) {
                notify("Error ${err}")
                currentBuild.result = 'FAILURE'
            }

            stage('archive') {
                step([$class: 'JUnitResultArchiver', testResults: 'target/surefire-reports/TEST-*.xml'])
                archiveArtifacts "target/*.?ar"
            }

            notify('Done')
        }
    }
}

def notify(status) {
    emailext body: """
        <p>${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
        <p>Check console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME} [${env.BUILD_NUMBER}]</a></p>
    """,
    subject: "${status}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
    to: 'bill.wilson@hyland.com'
}