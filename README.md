# Simple project to demostation deploy java application to k8s using helm.

#### app folder- contain the java spring boot with maven with a single action controller that return the free memory usage in container.

#### helm folder- contain the helm chart package with default template for k8s.

#### Jenkinsfile- the pipeline to build push and deploy the image to k8s.