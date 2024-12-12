# Github actions

- [GitHub Actions in 5 minutes or less](https://docs.github.com/en/actions/writing-workflows/quickstart)
 -GitHub Actions is a continuous integration and continuous delivery (CI/CD) platform that allows you to automate your build, test, and deployment pipeline.  


- [Sample YAML files for CI](https://github.com/actions/starter-workflows/tree/main/ci)
 -The repo is from Github carrying sample yaml file for various languages.  

- [Sample YAML files for CD](https://github.com/actions/starter-workflows/tree/main/deployments)
 -The repo is from Github carrying sample yaml file for deployment of project to various cloud platforms like AWS, Azure, GCP, Alibabacloud.

- [Sample YAML files for various code scanning platforms](https://github.com/actions/starter-workflows/tree/main/code-scanning)
 -The repo is from Github carrying sample yaml file for code scanning of project to various platforms like Sonarqube, Trivy, jfrog.

- [Workflow syntax for GitHub Actions](https://docs.github.com/en/actions/writing-workflows/workflow-syntax-for-github-actions#on)
 -Github documentation to understand syntax. 

- [Publishing Docker images to Docker Hub using GitHub Actions](https://docs.github.com/en/actions/use-cases-and-examples/publishing-packages/publishing-docker-images)
 -Github documentation to understand syntax.

 ```Yaml
name: Publish Docker image

on: [push]

jobs:
  push_to_registry:
    name: Push Docker image to Docker Hub
    runs-on: ubuntu-latest
    permissions:
      packages: write
      contents: read
      attestations: write
      id-token: write
    steps:
      - name: Check out the repo
        uses: actions/checkout@v4

      - name: Log in to Docker Hub
        uses: docker/login-action@f4ef78c080cd8ba55a85445d5b36e214a81df20a
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Extract metadata (tags, labels) for Docker
        id: meta
        uses: docker/metadata-action@9ec57ed1fcdbf14dcef7dfbe97b2010124a938b7
        with:
          images: <mention_your-docker-hub-repository_link>

      - name: Build and push Docker image
        id: push
        uses: docker/build-push-action@3b5e8027fcad23fda98b2e3ac259d8d67585f671
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
 ``` 

# Setup self-hosted runners

[LINK](https://www.youtube.com/watch?v=Rb2pUKdmdYo)
