stage('Deploy') {
  steps {
    kubernetesDeploy(
      kubeconfigId: 'config_4',
      configs: 'helloweb-deployment.yaml\svc.helloweb.yaml',
      enableConfigSubstitution: true,
      containerName: 'hello-app',
      containerImage: 'us-docker.pkg.dev/google-samples/containers/gke/hello-app:1.0',
      namespace: 'default',
      yamlMergeStrategy: 'merge'
    )
  }
}
