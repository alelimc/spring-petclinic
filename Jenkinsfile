pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/alelimc/spring-petclinic.git',
                    branch: 'main' // Specify the branch to build
                )
            }
        }

        stage('Build') {
            steps {
                sh './mvnw clean package'
            }
            post {
                success {
                    echo 'Build stage completed successfully.'
                }
                failure {
                    echo 'Build stage failed.'
                }
            }
        }

        stage('Test with JaCoCo') {
            steps {
                sh './mvnw test'
            }
            post {
                success {
                    jacoco(
                        execPattern: '**/target/jacoco.exec',
                        classPattern: '**/target/classes',
                        sourcePattern: '**/src/main/java',
                        exclusionPattern: '**/target/generated-sources/**'
                    )
                    echo 'Test stage completed successfully. JaCoCo report generated.'
                }
                failure {
                    echo 'Test stage failed.'
                }
            }
        }
    }

    triggers {
        cron('H/3 * * * 1') // Every 3 minutes on Mondays
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
        always {
            echo 'Cleaning up workspace...'
            cleanWs() // Clean up the workspace after the build
        }
    }
}
