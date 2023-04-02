pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/master']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    submoduleCfg: [],
                    userRemoteConfigs: [[url: 'https://github.com/dika-577/chatgpt.git']]
                ])
            }
        }
        stage('Deploy') {
            steps {
                withKubeConfig([credentialsId: 'config_4', contextName: 'minikube']) {
                    sh 'kubectl apply -f deployment.yaml'
                    sh 'kubectl apply -f service.yaml'
                }
            }
        }
        stage('Test') {
            steps {
                withKubeConfig([credentialsId: 'config_4', contextName: 'minikube']) {
                    script {
                        ip = "192.168.49.2"
                        def port = sh(returnStdout: true, script: "kubectl get service helloweb -o=jsonpath='{.spec.ports[0].nodePort}'").trim()
                        sh "kubectl rollout status deployment/helloweb"
                        sh "curl --fail --silent --show-error http://${ip}:${port}"
                    }
                }
            }
        }
    }
}
