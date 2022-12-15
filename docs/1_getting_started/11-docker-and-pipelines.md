# Docker & Pipelines

- [Docker \& Pipelines](#docker--pipelines)
  - [Docker in the Remote Desktop](#docker-in-the-remote-desktop)
  - [Dockerfile](#dockerfile)
  - [Pushing to the Container Registry](#pushing-to-the-container-registry)
  - [Using Your Docker Image in a Pipeline](#using-your-docker-image-in-a-pipeline)
    - [Couler](#couler)
    - [Hera Workflows](#hera-workflows)

## Docker in the Remote Desktop

Docker is running in a sidecar to the remote desktop. 
What this means is that we will need to connect to the 
Docker daemon on the local network.
We can accomplish this by opening up a terminal in the
Remote Desktop and setting the Environment variable 
as follows: 

```bash
export DOCKER_HOST=tcp://localhost:2375
```

Alternatively, you can run each docker command as follows:

```bash
docker command -H tcp://localhost:2375 ...
```

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

- `project-name` : The name of your project in the Container Registry
- `image-name` : The name that describes the Container Image
- `image-tag` : The version of the Image being built

Using the above information, replace the keywords in 
the following Docker commands

```bash
docker login registry.eo4ph.geoanalytics.ca
```

Next, we must build and tag our Container Image 
so that it will push to the correct registry:

```bash
docker build -t registry.eo4ph.geoanalytics.ca/project-name/image-name:image-tag .
```

Finally, we can push the successfully built Container Image:

```bash
docker push registry.eo4ph.geoanalytics.ca/project-name/image-name:image-tag
```

## Using Your Docker Image in a Pipeline

### Couler 

```python

```

### Hera Workflows