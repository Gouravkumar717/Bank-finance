pipeline {
    agent any
    tools {
        maven "M2_HOME" // Ensure this matches the name configured in Jenkins for Maven
    }
    stages {
        stage ("Clone and Build") {
            steps {
                // Cloning the repository
                git branch: "master", url: "https://github.com/Gouravkumar717/Bank-finance.git", credentialsId: "git-cread"
                
                // Running Maven build
                sh "mvn -Dmaven.test.failure.ignore=true clean package"pipeline {
    agent any
    tools {
        maven "M2_HOME"
    }
    stages {
        stage ("Clone and Build") {
            step {
                git branch: "master", url: "https://github.com/Gouravkumar717/Bank-finance.git", credentialsId: "git-cread"
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage ("Generate Test Reports") {
            steps {
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/Banking-Finance-project/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
            }
        }
        stage ("Create Docker Image") {
            step {
                sh "docker build -t gouragourav787/banking-finace-project:1.0"
            }
        }
    }
}
            }
        }
    }
}

