pipeline {
    agent any
 
    stages {
        stage('SCM Checkout') {
            steps {
                // Checkout the code from the Git repository
                git branch: 'master', url: 'https://github.com/Princepal1999/simple-gradle-java-app.git'
                script {
                    // Retrieve the latest commit hash from the repository
                    tag = sh(script: "git log -n 1 --pretty=format:'%H'", returnStdout: true).trim()
                    echo "Latest commit hash: ${tag}"
                }
            }
        }
 
        stage('Initialize Gradle') {
            steps {
                script {
                    // Initialize a new Gradle project with specified settings
                    def gradleInit = sh(script: '''
                        echo "Starting Gradle initialization"
                        gradle init --type application --language java --java-version 17 --dsl groovy --new-api
                    ''', returnStdout: true).trim()
                    echo "Gradle initialization output:"
                    echo "${gradleInit}"
                }
            }
        }
 
        stage('Run Application') {
            steps {
                script {
                    // Check if a Gradle build file exists before running the application
                    if (fileExists('build.gradle') || fileExists('build.gradle.kts')) {
                        // Run the Gradle application
                        sh './gradlew run'
                    } else {
                        // Fail the build if no Gradle build file is found
                        error 'No Gradle build file found!'
                    }
                }
            }
        }
 
        stage('Run Unit Tests') {
            steps {
                script {
                    // Check if a Gradle build file exists before running tests
                    if (fileExists('build.gradle') || fileExists('build.gradle.kts')) {
                        // Run Gradle unit tests
                        sh './gradlew test'
                    } else {
                        // Fail the build if no Gradle build file is found
                        error 'No Gradle build file found!'
                    }
                }
            }
        }
 
        stage('Generate Test Report') {
            steps {
                script {
                    // Define the directory and file for the test report
                    def reportDir = 'build/reports/tests/test'
                    def reportFile = "${reportDir}/index.html"
                    // Check if the test report file exists
                    if (fileExists(reportFile)) {
                        // Archive the test report for review
                        archiveArtifacts artifacts: reportFile, allowEmptyArchive: true
                    } else {
                        // Log a message if no test report is found
                        echo 'No test report found!'
                    }
                }
            }
        }
    }
 
    post {
        success {
            // Log a message on successful pipeline completion
            echo 'Pipeline completed successfully!'
        }
        failure {
            // Log a message on pipeline failure
            echo 'Pipeline failed!'
        }
        always {
            // Clean up the workspace after the pipeline completes
            cleanWs()
        }
    }
}

has context menu

