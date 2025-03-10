pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "kaivshah18/surveyform"
        BUILD_TIMESTAMP = new Date().format("yyyyMMdd-HHmmss", TimeZone.getTimeZone('UTC'))
        KUBECONFIG = "/var/lib/jenkins/.kube/config"
        RANCHER_NAMESPACE = "default"
        RANCHER_DEPLOYMENT = "hw2-deployment"
    }
    
    stages {
        stage("Checkout Code") {
            steps {
                script {
                    checkout scm
                }
            }
        }
        
        stage("Building Student Survey Application Image") {
            steps {
                script {
                    sh 'echo "Building Docker image..."'
                    sh "docker login -u kaivshah18 -p Pmnskskymd@18"

                    // Build Docker image with a timestamped tag
                    def customImage = docker.build("${DOCKER_IMAGE}:${BUILD_TIMESTAMP}", "--build-arg BUILD_ID=${BUILD_TIMESTAMP} .")
                }
            }
        }
        
        stage("Pushing Image to DockerHub") {
            steps {
                script {
                    sh "docker push ${DOCKER_IMAGE}:${BUILD_TIMESTAMP}"
                }
            }
        }
        
        stage("Deploying to Rancher") {
            steps {
                script {
                    // sh "kubectl set image deployment/${RANCHER_DEPLOYMENT} container-0=${DOCKER_IMAGE}:${BUILD_TIMESTAMP} -n ${RANCHER_NAMESPACE}"
                    // sh 'kubectl set image deployment/assign2-deployment container-0=amisha1407/amishafinalnew:${BUILD_TIMESTAMP} -n default'
                    sh "kubectl set image deployment/hw2-deployment container-0=${DOCKER_IMAGE} --namespace=default"
                    // sh "kubectl set image deployment/assign2-deployment container-0=amisha1407/amishafinalnew:${BUILD_TIMESTAMP} -n default --insecure-skip-tls-verify"
                    sh "kubectl rollout restart deployment/${RANCHER_DEPLOYMENT} -n ${RANCHER_NAMESPACE}"
                }
            }
        }
    }
    
    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed. Check Logs."
        }
    }
}
// pipeline {
//     agent any

//     environment {
//         DOCKER_IMAGE = "kaivshah18/surveyform"
//         TIMESTAMP = new Date().format("yyyyMMdd-HHmmss", TimeZone.getTimeZone('UTC'))
//         KUBECONFIG = "/var/lib/jenkins/.kube/config"
//         RANCHER_NAMESPACE = "default"
//         RANCHER_DEPLOYMENT = "hw2-cluster-deployment"
//     }
    
//     stages {
//         stage("Checkout Code") {
//             steps {
//                 script {
//                     checkout scm
//                 }
//             }
//         }
        
//         stage("Building Docker Image") {
//             steps {
//                 script {
//                     sh 'echo "Building Docker image..."'
//                     sh "docker login -u kaivshah18 -p 'Pmnskskymd@18'"
                    
//                     // Build Docker images with timestamped and latest tags
//                     sh "docker build -t ${DOCKER_IMAGE}:${TIMESTAMP} -t ${DOCKER_IMAGE}:01 ."
//                 }
//             }
//         }
        
//         stage("Pushing Image to DockerHub") {
//             steps {
//                 script {
//                     // Push both the timestamped and latest tag
//                     sh "docker push ${DOCKER_IMAGE}:${TIMESTAMP}"
//                     sh "docker push ${DOCKER_IMAGE}:01"
//                 }
//             }
//         }
        
//         stage("Deploying to Rancher") {
//             steps {
//                 script {
//                     // Update deployment to use the timestamped image
//                     sh "kubectl set image deployment/${RANCHER_DEPLOYMENT} container-0=${DOCKER_IMAGE}:${TIMESTAMP} --namespace=${RANCHER_NAMESPACE}"
//                     sh "kubectl rollout restart deployment/${RANCHER_DEPLOYMENT} -n ${RANCHER_NAMESPACE}"
//                 }
//             }
//         }
//     }
    
//     post {
//         success {
//             echo "✅ Deployment Successful!"
//         }
//         failure {
//             echo "❌ Deployment Failed. Check Logs."
//         }
//     }
// }
