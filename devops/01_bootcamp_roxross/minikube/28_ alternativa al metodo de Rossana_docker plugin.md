To securely run Docker commands in your Jenkins pipeline without using Docker-in-Docker (DinD), you can use the Docker Pipeline plugin. This plugin provides a built-in `docker` global variable that you can use to run Docker commands in your Jenkinsfile.

Here's an example of how you can use the Docker Pipeline plugin to build and run a Docker image:

```groovy
pipeline {
    agent any

    stages {
        stage('Build and Test') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                        def customImage = docker.build("my-image:${env.BUILD_ID}")

                        customImage.inside {
                            sh 'make test'
                        }
                    }
                }
            }
        }
    }
}
```

In this example, the `docker.build` method is used to build a Docker image, and the `inside` method is used to run commands inside a Docker container.

Before you can use the Docker Pipeline plugin, you'll need to install it:

1. Go to Jenkins Dashboard -> Manage Jenkins -> Manage Plugins.
2. Click on the Available tab and search for "Docker Pipeline".
3. Check the box next to it and click on Install without restart.

After the plugin is installed, you'll need to configure it:

1. Go to Jenkins Dashboard -> Manage Jenkins -> Configure System.
2. Scroll down to the Cloud section and click on Add a new cloud -> Docker.
3. Configure the Docker host URI and other settings as needed.

Please note that the Docker host needs to be accessible from the Jenkins agent nodes. If your Jenkins is running inside a Docker container, you'll need to expose the Docker socket to the Jenkins container by adding `-v /var/run/docker.sock:/var/run/docker.sock` to your `docker run` command. However, this approach has security implications and should be used with caution.

==running Jenkins from the hostOS:

Yes, installing Jenkins directly on your host OS is another alternative. In this setup, Jenkins will have direct access to the Docker daemon running on the host, so you can run Docker commands directly in your Jenkins pipeline without needing to use Docker-in-Docker (DinD).

In this case, you can still use the Docker Pipeline plugin, but it's not strictly necessary. The Docker Pipeline plugin provides a convenient way to define Docker-based build environments in your Jenkinsfile, but you can also run Docker commands directly using the `sh` step.

Here's an example of how you can run Docker commands directly in your Jenkins pipeline:

```groovy
pipeline {
    agent any

    stages {
        stage('Build and Test') {
            steps {
                sh 'docker build -t my-image .'
                sh 'docker run --name my-container -d my-image'
                sh 'docker exec my-container make test'
            }
        }
    }
}
```

In this example, the `sh` step is used to run Docker commands directly. The Docker daemon running on the host will execute these commands.

Please note that you'll need to ensure that the Jenkins user has permission to access the Docker daemon on the host. You can do this by adding the Jenkins user to the `docker` group on the host. However, this approach has security implications and should be used with caution.

