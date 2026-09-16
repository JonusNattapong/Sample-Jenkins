# Sample-Jenkins

Build Bloom is a small static website deployed by Jenkins.

The `Jenkinsfile` validates the page, builds a Docker image, deploys it as a container, and verifies the running container.

The pipeline builds a Docker image named `sample-jenkins:<build-number>` and serves it internally on port `8081`.

Open the deployed site at `http://192.168.88.178/sample-jenkins/`.
