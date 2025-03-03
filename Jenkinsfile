    pipeline {
        agent any

        stages {
            stage('Checkout') {
                steps {
                    git 'https://github.com/alelimc/spring-petclinic.git'
                }
            }

            stage('Build') {
                steps {
                    sh './mvnw clean package'
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
                    }
                }
            }
        }

        triggers {
            cron('H/3 * * * 1') // Every 3 minutes on Mondays (1st day of the week)
        }
    }