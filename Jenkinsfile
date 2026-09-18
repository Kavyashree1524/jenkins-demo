pipeline {

    // Jenkins can run this pipeline on any available agent
    agent any

    // Parameters: values entered when starting the build
    parameters {
        choice(
            name: 'ENVIRONMENT',
            choices: ['DEV', 'QA', 'PROD'],
            description: 'Select environment'
        )

        booleanParam(
            name: 'RUN_TESTS',
            defaultValue: true,
            description: 'Run automated tests?'
        )

        string(
            name: 'VERSION',
            defaultValue: '1.0.0',
            description: 'Application version'
        )
    }

    // Environment variables
    environment {
        APP_NAME = 'Jenkins-Demo'
        BUILD_VERSION = "${params.VERSION}"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Checking out source code..."
                echo "Application: ${APP_NAME}"
                echo "Version: ${BUILD_VERSION}"

                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building application..."
                sh 'echo Build successful'
            }
        }

        // Run this stage only when RUN_TESTS is true
        stage('Testing') {
            when {
                expression {
                    return params.RUN_TESTS
                }
            }

            parallel {

                stage('API Tests') {
                    steps {
                        echo "Running API tests..."
                        sh 'echo API tests completed'
                    }
                }

                stage('UI Tests') {
                    steps {
                        echo "Running UI tests..."
                        sh 'echo UI tests completed'
                    }
                }

                stage('Security Tests') {
                    steps {
                        echo "Running security tests..."
                        sh 'echo Security tests completed'
                    }
                }
            }
        }

        // Run only for QA environment
        stage('QA Deployment') {
            when {
                expression {
                    return params.ENVIRONMENT == 'QA'
                }
            }

            steps {
                echo "Deploying to QA..."
                sh 'echo QA deployment successful'
            }
        }

        // Run only for PROD
        stage('Production Approval') {
            when {
                expression {
                    return params.ENVIRONMENT == 'PROD'
                }
            }

            steps {
                input message: 'Do you want to deploy to Production?',
                      ok: 'Deploy'
            }
        }

        stage('Production Deployment') {
            when {
                expression {
                    return params.ENVIRONMENT == 'PROD'
                }
            }

            steps {
                echo "Deploying to Production..."
                sh 'echo Production deployment successful'
            }
        }

        // Demonstrates catchError
        stage('Optional Check') {
            steps {
                catchError(
                    buildResult: 'SUCCESS',
                    stageResult: 'UNSTABLE'
                ) {
                    echo "Running optional check..."

                    // Example failure
                    sh 'exit 1'
                }

                echo "Pipeline continues even if Optional Check fails."
            }
        }
    }

    // Actions after pipeline completion
    post {

        success {
            echo "Pipeline completed successfully."
        }

        failure {
            echo "Pipeline failed. Check the console log."
        }

        unstable {
            echo "Pipeline is UNSTABLE."
        }

        always {
            echo "Pipeline execution completed."
        }
    }
}