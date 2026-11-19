
node {
    
    // Configure JDK and Maven
    def jdkHome = tool 'JDK'
    def mavenHome = tool 'Maven'

    env.JAVA_HOME = jdkHome
    env.PATH = "${jdkHome}\\bin;${mavenHome}\\bin;${env.PATH}"

    stage('Checkout') {
        echo 'Downloading Blood Bank source code'

        git branch: 'main',
            url: 'https://github.com/sarasrija/BloodBanksystem.git'
    }

    stage('Clean') {
        echo 'Cleaning previous build files'
        bat 'mvn clean'
    }

    stage('Install') {
        echo 'Compiling and installing project'
        bat 'mvn install -DskipTests'
    }

    stage('Test') {
        echo 'Running unit tests'

        catchError(buildResult: 'UNSTABLE',
                   stageResult: 'FAILURE') {
            bat 'mvn test'
        }
    }

    stage('Package') {
        echo 'Creating WAR package'
        bat 'mvn package -DskipTests'
    }

    stage('Archive') {
        echo "Build Number: ${env.BUILD_NUMBER}"

        archiveArtifacts artifacts: 'target/*.war',
                         fingerprint: true
    }
}
