pipeline {
    agent {
        docker {
            image 'maven:3-alpine' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -B -DskipTests clean package' 
            }
        }
	stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
	stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
    post {
        success {
            mail to: 'shawn.oplinger@noaa.gov',
                 from: 'jenkins.builds.nmfs.cio@noaa.gov',
		 subject: "Success Pipeline: ${currentBuild.fullDisplayName}",
                 body: "Success for build: ${env.BUILD_URL}"
        }
        failure {
            mail to: 'shawn.oplinger@noaa.gov',
                 from: 'jenkins.builds.nmfs.cio@noaa.gov',
		 subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                 body: "Something is wrong with ${env.BUILD_URL}"
        }
    }
}
