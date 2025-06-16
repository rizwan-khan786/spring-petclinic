pipeline {
    agent any

    environment {
        JAVA_HOME   = '/usr/lib/jvm/java-17-openjdk-amd64'
        MAVEN_HOME  = '/usr/share/maven'
        PATH        = "${JAVA_HOME}/bin:${MAVEN_HOME}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo "Building branch: ${env.BRANCH_NAME}"
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                echo "Deploying production build..."
                // Insert deployment commands here (scp, aws CLI, etc.)
            }
        }
    }

    post {
        success {
            echo "✅ Build succeeded on branch ${env.BRANCH_NAME}"
        }
        failure {
            echo "❌ Build failed on branch ${env.BRANCH_NAME}"
        }
    }
}
