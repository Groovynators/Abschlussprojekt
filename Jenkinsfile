pipeline {
    agent any
    stages {
		stage('Compile') {
			steps {
                script {
                    mvn.compile()
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    mvn.test()
                }
            }
        }
        stage('Sonar Verify') {
            steps {
                script {
                    mvn.verify()
                }
            }
        }
        stage('Package') {
            steps {
                script {
                    mvn.artifactpackage()
                }
            }
        }
        stage('Deploy Nexus') {
            steps {
                configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]){
                script {
                    mvn.deploy()
                    }
                }
            }
        }
        stage('Deploy Tomcat') {
			steps {
                configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]){
                script {
                    mvn.tomcat()
                    }
                }
            }
        }
    }
}