
node {

    // Configure JDK and Maven
    def jdkHome = tool 'JDK'
    def mavenHome = tool 'Maven'

    env.JAVA_HOME = jdkHome
    env.PATH = "${jdkHome}\\bin;${mavenHome}\\bin;${env.PATH}"

    try {

        stage('Checkout') {
            echo 'Checking out Blood Bank Management System'

            git branch: 'main',
                url: 'https://github.com/varanasi-sneha/internalpracticetoday.git'
        }

        stage('Clean') {
            echo 'Cleaning previous build files'
            bat 'mvn clean'
        }

        stage('Install') {
            echo 'Compiling and installing the project'
            bat 'mvn install -DskipTests'
        }

        stage('Test') {
            echo 'Executing unit tests'

            catchError(
                buildResult: 'UNSTABLE',
                stageResult: 'FAILURE'
            ) {
                bat 'mvn test'
            }
        }

        stage('Package') {
            echo 'Packaging Blood Bank application'
            bat 'mvn package -DskipTests'
        }

        stage('Archive') {
            echo "Build Number: ${env.BUILD_NUMBER}"

            archiveArtifacts(
                artifacts: 'target/*.war',
                fingerprint: true
            )
        }

    } catch (Exception e) {

        echo "Pipeline failed: ${e.getMessage()}"
        currentBuild.result = 'FAILURE'
        throw e

    } finally {

        echo "Blood Bank Pipeline execution completed."
        echo "Build Number: ${env.BUILD_NUMBER}"
        echo "Build Status: ${currentBuild.currentResult}"

    }
}
