pipeline{
 environment {
        registryCredentials = "dockerhub_id"
        dockerImageReact = ""
        dockerImageSpring = ""
    }
    agent any
        stages {
            stage('Checkout React App') {
                steps {
                    dir('lbg-car-react-starter') {
                        git branch: 'main', url: 'https://github.com/troycdev/lbg-car-react-starter'
                    }
                }
            }

            stage('Checkout Spring App') {
                steps {
                    dir('lbg-car-spring-app-starter') {
                        git branch: 'main', url: 'https://github.com/troycdev/lbg-car-spring-app-starter'
                    }
                }
            }

            stage ('Build React Docker Image'){
                steps{
                    script {
                        dockerImageReact = docker.build("troycooper/lbg-car-react-starter", "./lbg-car-react-starter")
                    }
                }
            }

            stage ("Push React Image to Docker Hub"){
                steps {
                    script {
                        docker.withRegistry('', registryCredentials) {
                            dockerImageReact.push("${env.BUILD_NUMBER}")
                            dockerImageReact.push("latest")
                        }
                    }
                }
            }

            stage ('Build Spring Docker Image'){
                steps{
                    script {
                        dockerImageSpring = docker.build("troycooper/lbg-car-spring-app-starter", "./lbg-car-spring-app-starter")
                    }
                }
            }

            stage ("Push Spring Image to Docker Hub"){
                steps {
                    script {
                        docker.withRegistry('', registryCredentials) {
                            dockerImageSpring.push("${env.BUILD_NUMBER}")
                            dockerImageSpring.push("latest")
                        }
                    }
                }
            }

            stage ("Clean up"){
                steps {
                    script {
                        sh 'docker image prune --all --force --filter "until=48h"'
                           }
                }
            }
        }
}
