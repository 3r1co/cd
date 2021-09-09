# Github Actions (1 hour)

The goal of this exercise is to show you how to create a CI/CD Pipeline for containerized applications.  
For this Lab, we will use the automation platform of Github, called Github Actions.

At the end of the exercise, should have added a Github Actions YAML file to your repository and on each commit and push to the master branch, a new Docker image should be built automatically.

Find below an example for a Github Actions pipeline.
This file must be placed under `.github/workflows/build.yaml`:

    name: Build App

    on:
        push:
            branches:
            - main

    jobs:

        build:
        
            runs-on: ubuntu-latest
        
            steps:
            - uses: actions/checkout@v2
            - name: build-push
            uses: docker/build-push-action@v1
            with:
                username: ${{ secrets.DOCKER_USERNAME }}
                password: ${{ secrets.DOCKER_PASSWORD }}
                registry: docker.io
                repository: <your-dockerhub-username>/cd
                tag_with_sha: true

Take care that you need to specify your username and password in the project secrets according to this documentation: https://docs.github.com/en/actions/reference/encrypted-secrets
The environment variable for your username should be `DOCKER_USERNAME`, the one for your password `DOCKER_PASSWORD`.