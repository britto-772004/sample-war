pipeline {
    agent any


    stages {
        stage('Clone Repository') {
            steps {
                git branch: "dev" , url: "https://github.com/britto-772004/sample-war.git"
            }
        }

        stage('Build with Maven') {
            steps {
                // sh "/usr/share/maven/bin/mvn clean package"
                // sh "mvn validate"
                // sh "mvn test"
                sh "/usr/share/maven/bin mvn package"
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    def warFile = findFiles(glob: 'target/*.war')[0].path
                    echo "Deploying WAR file: ${warFile} to http://localhost:8000/britto"

                    sh """
                        curl -u admin:admin123 \\
                        -T ${warFile} \\
                        "http://localhost:8000/manager/text/deploy?path=/britto&update=true"
                    """
                }
            }
        }
    }

    post {
        success {
            echo "Deployment successful! Access your app at: http://localhost:8000/britto"
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
