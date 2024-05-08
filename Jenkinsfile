pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Stage 1: Build- Compiles source code and packages it into an executable or distributable format.'
                echo 'Tool used: Maven- a build automation tool used primarily for Java projects.'
                // Assuming Maven is used to compile and package the application
                script {
                    sh 'mvn clean package'
                }
            }
        }
        stage('Unit and Integration Tests') {
            steps {
                echo 'Stage 2: Unit and Integration Tests- Runs automated tests to verify the functionality of individual units of code and their interaction.'
                echo 'Tool used: JUnit for unit testing and Selenium for integration testing.'
                // Run unit and integration tests using Maven
                script {
                    sh 'mvn test'
                }
            }
            post {
                always {
                    // Archive logs, assuming they are generated in the 'build/logs' directory
                    archiveArtifacts allowEmptyArchive: true, artifacts: '**/build/logs/*.log', fingerprint: true
                    // Email notifications with attachment of logs
                    emailext (
                        to: 'mouna200022@gmail.com',
                        subject: "Jenkins - Test Stage: ${currentBuild.currentResult}",
                        body: """<p>Stage completed with status: ${currentBuild.currentResult}</p>
                                 <p>Please find attached the logs for more details.</p>""",
                        attachmentsPattern: '**/build/logs/*.log'
                    )
                }
            }
        }
        stage('Code Analysis') {
            steps {
                echo 'Stage 3: Code Analysis- Analyzes the source code for potential bugs and anti-patterns to ensure quality and maintainability.'
                echo 'Tool used: SonarQube- provides continuous inspection of code quality.'
                // Assuming SonarQube is configured to work with Maven
                script {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Security Scan') {
            steps {
                echo 'Stage 4: Security Scan- Scans the code, dependencies, and configurations for known security vulnerabilities.'
                echo 'Tool used: OWASP Dependency Check- identifies project dependencies and checks if there are any known, publicly disclosed vulnerabilities.'
                // Example script call for a security scanning tool
                script {
                    sh 'run-security-scan.sh'
                }
            }
            post {
                always {
                    // Handle security scan artifacts
                    archiveArtifacts allowEmptyArchive: true, artifacts: '**/security-reports/*.log', fingerprint: true
                    emailext (
                        to: 'mouna200022@gmail.com',
                        subject: "Jenkins - Security Scan: ${currentBuild.currentResult}",
                        body: """<p>Stage completed with status: ${currentBuild.currentResult}</p>
                                 <p>Please find attached the logs for more details.</p>""",
                        attachmentsPattern: '**/security-reports/*.log'
                    )
                }
            }
        }
        stage('Deploy to Staging') {
            steps {
                echo 'Stage 5: Deploy to Staging- Deploys the application to a staging environment where it can be tested in a production-similar setup.'
                echo 'Tool used: Jenkins SSH Plugin- allows the automation of commands over SSH, useful for deployment.'
                // Deploy to staging using SSH
                script {
                    sh 'deploy-to-staging.sh'
                }
            }
        }
        stage('Integration Tests on Staging') {
            steps {
                echo 'Stage 6: Integration Tests on Staging- Runs integration tests in the staging environment to validate the application against the production-like setup.'
                echo 'Tool used: Selenium- for UI testing.'
                // Run integration tests on staging environment
                script {
                    sh 'run-integration-tests-staging.sh'
                }
            }
        }
        stage('Deploy to Production') {
            steps {
                echo 'Stage 7: Deploy to Production- Deploys the application to the production environment where it becomes accessible to end-users.'
                echo 'Tool used: Jenkins SSH Plugin- used for executing remote commands necessary for deploying applications.'
                // Deploy to production using SSH
                script {
                    sh 'deploy-to-production.sh'
                }
            }
        }
    }
    post {
        always {
            echo 'General notification: The pipeline has completed execution.'
        }
    }
}
