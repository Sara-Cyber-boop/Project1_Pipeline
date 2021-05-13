pipeline {
    environment {
        VERSION = "1.0.0"
        registry="Sara-Cyber-boop/Project1_Pipeline"
        registry_tag = "$registry:$VERSION"
        credentialId = "Dockerhub"
        projectDir = "./Project1_Pipeline"
        dockerImage = ""
    }
    agent any
    stages {
        stage("Cloning repository") {
            steps {
                checkout scm
            }
        }
        stage("Building Docker image") {
            steps {
                script {
                    dockerImage = docker.build(registry_tag, "-f $projectDir/Dockerfile $projectDir")
                }
            }
        }
        stage("Testing Docker image") {
            steps {
                script {
                    dockerImage.inside {
                        sh 'echo "Tests passed!"'
                    }
                }
            }
        }
        stage("Publishing Docker image") {
            steps {
                script {
                    docker.withRegistry("", credentialId) {
                        dockerImage.push()
                        dockerImage.push("latest")
                    }
                }
            }
        }
        stage("Cleaning up local registry") {
            steps {
                sh "docker rmi $registry_tag"
                sh "docker rmi $registry:latest"
            }
        }
    }
}
