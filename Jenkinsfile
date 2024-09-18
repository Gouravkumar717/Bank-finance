pipeline {
    agent any
    tools {
        maven "M2_HOME"
    }
    stages {
        stage("Clone and Build") {
            steps {
                git branch: "master", url: "https://github.com/Gouravkumar717/Bank-finance.git", credentialsId: "git-cread"
                sh "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }
        stage("Generate Test Reports") {
            steps {
                publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report'])
            }
        }
        stage("Create Docker Image") {
            steps {
                sh "docker build -t gourav787/banking-finance-project:1.0 ."
            }
        }
        stage("Docker-login") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'Docker-cred', passwordVariable: 'dockerpassword', usernameVariable: 'dockerlogin')]) {
                    sh "docker login -u ${dockerlogin} -p ${dockerpassword}"
                }
            }
        }
        stage("Docker-push") {
            steps {
                sh "docker push gourav787/banking-finance-project:1.0"
            }
        }
        stage('Deploy with Terraform') {
            steps {
                withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'AwsAccessKey', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
                    dir("terraform-files") {
                        sh 'chmod 600 mykeyohio.pem' // Ensure the permissions are set for the key
                        sh 'terraform init'
                        sh 'terraform validate'
                        sh 'terraform apply --auto-approve'
                    }
                }
            }
        }
    }
}
