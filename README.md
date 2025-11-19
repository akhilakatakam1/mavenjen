pipeline {
    agent any
    tools {
        maven 'MAVEN_HOME'   // must match your Maven installation name in Jenkins Global Tool Config
    }
    stages {
        stage('git repo & clean') {
            steps {
                bat "rmdir /s /q mavenjen || exit 0"   // remove old checkout if exists (Windows safe)
                bat "git clone https://github.com/kpavithrachowdary/mavenjen.git"
                dir('mavenjen') {
                    bat "mvn clean"
                }
            }
        }
        stage('install') {
            steps {
                dir('mavenjen') {
                    bat "mvn install"
                }
            }
        }
        stage('test') {
            steps {
                dir('mavenjen') {
                    bat "mvn test"
                }
            }
        }
        stage('package') {
            steps {
                dir('mavenjen') {
                    bat "mvn package"
                }
            }
        }
    }
}
