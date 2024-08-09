pipeline {
    agent any

    tools {
        gradle 'Gradle'
    }

    stages {
        stage('SCM Checkout') {
            steps {
                // Checkout the code from the Git repository
                git branch: 'master', url: 'https://github.com/Princepal1999/simple-gradle-java-app.git'
                script {
                    // Retrieve and echo the latest commit hash
                    def latestCommitHash = sh(script: "git log -n 1 --pretty=format:'%H'", returnStdout: true).trim()
                    echo "Latest commit hash: ${latestCommitHash}"
                }
            }
        }

        stage('Initialize Gradle') {
            steps {
                script {
                    // Initialize the Gradle project
                    def gradleInitOutput = sh(script: '''
                        echo "Starting Gradle initialization"
                        gradle init --type application --language java --java-version 17 --dsl groovy --new-api
                    ''', returnStdout: true).trim()
                    echo "Gradle initialization output:"
                    echo "${gradleInitOutput}"
                }
            }
        }

        stage('Run Application') {
            steps {
                script {
                    // Check for Gradle build files and run the application
                    if (fileExists('build.gradle') || fileExists('build.gradle.kts')) {
                        sh './gradlew run'
                    } else {
                        error 'No Gradle build file found!'
                    }
                }
            }
        }

        stage('Run Unit Tests') {
            steps {
                script {
                    // Check for Gradle build files and run unit tests
                    if (fileExists('build.gradle') || fileExists('build.gradle.kts')) {
                        sh './gradlew test'
                    } else {
                        error 'No Gradle build file found!'
                    }
                }
            }
        }

        stage('Generate Test Report') {
            steps {
                script {
                    // Define paths for the test report
                    def reportDir = 'build/reports/tests/test'
                    def reportFile = "${reportDir}/index.html"

                    // Archive the test report if it exists
                    if (fileExists(reportFile)) {
                        archiveArtifacts artifacts: reportFile, allowEmptyArchive: true
                    } else {
                        echo 'No test report found!'
                    }
                }
            }
        }
    }

    post {
        success {
            // Notify on successful pipeline completion
            echo 'Pipeline completed successfully!'
        }
        failure {
            // Notify on pipeline failure
            echo 'Pipeline faile
