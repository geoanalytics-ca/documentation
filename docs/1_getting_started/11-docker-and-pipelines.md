# Docker & Pipelines

- [Docker \& Pipelines](#docker--pipelines)
  - [Docker in the Remote Desktop](#docker-in-the-remote-desktop)
  - [Dockerfile](#dockerfile)
  - [Pushing to the Container Registry](#pushing-to-the-container-registry)
  - [Using Your Docker Image in a Pipeline](#using-your-docker-image-in-a-pipeline)
    - [Couler](#couler)

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

Where `command` is a Docker command such as `images`, `ps`, etc.

## Dockerfile

To build a Container Image, once the application has
been implemented, you will need to sue a `dockerfile`
to allow the Container Engine to containerize your application. 

This `dockerfile` describes/builds the environment which 
your application will run in. 
It it highly recommend to keep this as slim and consice as necessary to run your application. 

```text
# /src/requirements.txt
xarray==2022.10.0
numpy==1.23.3
pandas==1.5.1
geopandas==0.12.1
```

```dockerfile
FROM python:3.10.9-slim

RUN apt update && apt install -y \
      software-properties-common

COPY src /app

RUN pip install -r /app/requirements.txt

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
def pipeline_job(name, source, env_user):
    toleration = Toleration('ga.nodepool/type', 'NoSchedule', 'Exists')
    couler.add_toleration(toleration)
    toleration2 = Toleration('kubernetes.azure.com/scalesetpriority', 'NoSchedule', 'Exists')
    couler.add_toleration(toleration2)
    image_secret = ImagePullSecret('harborregcred')
    couler.add_image_pull_secret(image_secret)
    return couler.run_script(
        image="registry.eo4ph.geoanalytics.ca/project-name/image-name:image-tag",
        step_name=name,
        source=source,
        env=env_user,
        node_selector={'pipeline':'small'},
        secret=image_secret
    )

# Some environment variables for the Job Task
env = {
  'some-env': 'env-value'
}

def user_function():
  # Do Something

def create_job():
    A = couler.set_dependencies(
        lambda: pipeline_job(name='job1', source=user_function, env_user=env),
        dependencies=None
    )

create_job()
submitter = ArgoSubmitter(namespace='pipeline')
deployment = couler.run(submitter=submitter)
deployment
```

<!-- ### Hera Workflows -->