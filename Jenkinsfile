pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Clone the repository
                    git branch: 'main', url: 'https://github.com/sm-dotcom/notes_da3.git'
                }
            }
        }

        stage('Static Code Analysis') {
            parallel {
                stage('Frontend: ESLint') {
                    steps {
                        script {
                            dir('frontend') {
                                sh 'npm install'  // Ensure dependencies are installed
                                sh 'npx eslint src --ext .js,.jsx'  // Run ESLint
                            }
                        }
                    }
                }

                stage('Backend: SonarQube') {
                    steps {
                        script {
                            withSonarQubeEnv('SonarQube') { // Ensure SonarQube is configured in Jenkins
                                dir('backend') {
                                    sh './gradlew sonarqube' // Example for Java backend
                                }
                            }
                        }
                    }
                }
            }
        }
    }

    post {
        failure {
            echo "Build failed due to static code issues!"
        }
    }
}
