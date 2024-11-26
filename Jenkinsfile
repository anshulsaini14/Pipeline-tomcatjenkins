pipeline {
    agent any
    tools {
        maven 'Maven-a'
    }
    triggers {
      pollSCM 'H/3 * * * *'
    }
    stages {
        stage("Test") {
            steps {
                sh "mvn test" // Runs Maven tests
            }
        }
        stage("Build") {
            steps {
                sh "mvn package" // Packages the application
            }
        }
        stage("Deploy on Test") {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'jenkins', path: '', url: 'http://172.16.9.129:8880')], contextPath: '/app-pipeline', war: '**/*.war'
            }
        }
        stage("Deploy on Prod") {
            input {
        message "Should we continue with production deployment?"
        ok "Yes, proceed."
        }
            steps {
                deploy adapters: [tomcat9(credentialsId: 'jenkins', path: '', url: 'http://172.16.9.128:8080')], contextPath: '/app-pipeline', war: '**/*.war'
            }
        }
    }
}
