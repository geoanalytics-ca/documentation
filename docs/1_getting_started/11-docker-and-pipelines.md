# Docker & Pipelines

- [Docker \& Pipelines](#docker--pipelines)
  - [Dockerfile](#dockerfile)
  - [Pushing to the Container Registry](#pushing-to-the-container-registry)
  - [Using Your Docker Image in a Pipeline](#using-your-docker-image-in-a-pipeline)

## Dockerfile

To build a Container Image, once the application has
been implemented, you will need to sue a `dockerfile`
to allow the Container Engine to containerize your application. 

This `dockerfile` describes/builds the environment which 
your application will run in. 
It it highly recommend to keep this as slim and consice as necessary to run your application. 

```dockerfile
FROM ubuntu:22.04-slim

COPY src /app

ENTRYPOINT ['/bin/bash', '-c', 'entryscript.sh']

```

## Pushing to the Container Registry

First, to ensure we will successfully push our built Container Image, we must login to our container registry with your provided username and password:

```bash
docker login registry.eo4ph.geoanalytics.ca
```

Next, we must build and tag our Container Image 
so that it will push to the correct registry:

```bash
docker build -t registry.eo4ph.geoanalytics.ca/ .
```

Finally, we can push the successfully built Container Image:

```bash
docker push registry.eo4ph.geoanalytics.ca/
```

## Using Your Docker Image in a Pipeline

```python

```