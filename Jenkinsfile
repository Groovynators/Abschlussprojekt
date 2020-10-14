pipeline {
    agent any
    environment {
        NEXUS_HOST = 'nexus:8081'
        SONAR_HOST = 'sonarqube:9000'
               }
        stages {
            stage('Docker-compose-up') {
                steps {
                    sh 'docker-compose up -d --build' 
                }
            }
        stages {
            stage('Testen und Kompilieren') {
                steps {
                    sh 'mvn clean test' 
                }
            }
           stage('Sonar Verify') {
                steps {
                    configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                       sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS clean verify sonar:sonar'
                    }
                }
            }
            stage('WAR-File erstellen') {
                steps {
                      sh 'mvn clean package -DskipTests'
                      sh 'ls -al target'
                }
            }
            stage('deploy to nexus') {
                steps {
                    withCredentials([usernamePassword(credentialsId: 'nexus', usernameVariable: 'NEXUS_USER', passwordVariable: 'NEXUS_PASSWORD')]) {
                    configFileProvider([configFile(fileId: 'default', variable: 'MAVEN_GLOBAL_SETTINGS')]) {
                        sh 'mvn -gs $MAVEN_GLOBAL_SETTINGS deploy'
                    }
                    }
                       } 
                    }
            stage("deploy War-file to tomcat") {
                steps {
                    ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'ansible', inventory: 'inventory', playbook: 'deploy.yml'
            }
        }
        stages {
            stage('Docker-compose down') {
                steps 
                sleep (300){
                    sh 'docker-compose down' 
                }
            }
                }
            }