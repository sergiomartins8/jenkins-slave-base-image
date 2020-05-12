# jenkins-slave-base

[![version](https://images.microbadger.com/badges/version/sergiomartins8/jenkins-slave-base.svg)](https://microbadger.com/images/sergiomartins8/jenkins-slave-base)
[![release](https://github.com/sergiomartins8/jenkins-slave-base/workflows/release/badge.svg)](https://github.com/sergiomartins8/jenkins-slave-base/actions?query=workflow%3Arelease)
[![layers](https://images.microbadger.com/badges/image/sergiomartins8/jenkins-slave-base.svg)](https://microbadger.com/images/sergiomartins8/jenkins-slave-base)
[![docker stars](https://img.shields.io/docker/stars/sergiomartins8/jenkins-slave-base.svg?style=flat)](https://hub.docker.com/r/sergiomartins8/jenkins-slave-base/)
[![docker pulls](https://img.shields.io/docker/pulls/sergiomartins8/jenkins-slave-base.svg)]()


## About

The goal is to build a useful image to serve as the base for Jenkins slaves. Thus, containing goods such as java, node, git, maven and python.
And, on top of that it provides kubectl and helm binaries for working with a Kubernetes cluster.

## Getting Started

### Pull the image

````shell script
$ docker pull sergiomartins8/jenkins-slave-base:latest
````

#### Jenkinsfile example

```groovy
podTemplate(label: 'jenkins-slave-base-pod', serviceAccount: 'jenkins', containers: [
    containerTemplate(
        name: 'base', 
        image: 'sergiomartins8/jenkins-slave-base:latest', 
        ttyEnabled: true, 
        command: 'cat'
    )
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
  ]
  ) {
    node('jenkins-slave-base-pod') {
        stage('Awesome stage') {
            container('base') {
                sh 'kubectl get po'
                sh 'helm env'
            }
        }
    }
}
```

> Try out the above example using the [jenkins-k8s-minikube](https://github.com/sergiomartins8/jenkins-k8s-minikube) project 🚀