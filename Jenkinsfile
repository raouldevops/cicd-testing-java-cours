def CONTAINER_NAME = "calculator"
def ENV_NAME = getEnvName(env.BRANCH_NAME)
def CONTAINER_TAG = getTag(env.BUILD_NUMBER,env.BRANCH_NAME)
def HTTP_PORT = getHTTPPort(env.BRANCH_NAME)
def EMAIL_RECIPIENTS = "cipahraoul@yahoo.fr"

node {
        stage('checkout') {
            checkout scm
        }

        stage('Build and test') {
            sh "mvn clean install"
        }

        stage("build & SonarQube analysis") {
           withSonarQubeEnv('sonarServer') {
             sh "mvn sonar:sonar -Dintegration-tests.skip=true -Dmaven.test.failure.ignore=true"
           }
           timeout(time: 1, unit: 'HOURS') {
            def qg = waitForQualityGate()
            if (qg.status != 'OK') {
                error "Pipeline aborted due to quality gate failure: ${qg.status}"
            }
           }
        }

}