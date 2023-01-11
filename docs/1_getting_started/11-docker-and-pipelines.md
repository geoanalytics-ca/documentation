# Docker & Pipelines

Pairing Docker with Pipelines enables us to create customized solutions 
that we have full control over. 
Defining environments to run applications inside a `dockerfile` and 
running them in an isolated space on the Pipelines engine allows 
for a secure and reproducible environment to run tests and/or 
(scheduled) reproducible workflows. 

- [Docker \& Pipelines](#docker--pipelines)
  - [Useful Links](#useful-links)
  - [Environtment Variables](#environtment-variables)
  - [Docker in the Remote Desktop](#docker-in-the-remote-desktop)
  - [Using Docker and Pipelines](#using-docker-and-pipelines)
  - [Example Project](#example-project)
  - [Dockerfile](#dockerfile)
  - [Building and Pushing to the Container Registry](#building-and-pushing-to-the-container-registry)
  - [Using Your Docker Image in a Pipeline](#using-your-docker-image-in-a-pipeline)
    - [Hera-Workflows](#hera-workflows)
    - [Couler](#couler)

## Useful Links

- [Docker Container Engine API](https://docs.docker.com/engine/api/)
- [Dockerfile Reference](https://docs.docker.com/engine/reference/builder/)
- [Hera-Workflows API & Examples](https://hera-workflows.readthedocs.io/en/latest/?badge=latest)
- [Couler Documentation](https://couler-proj.github.io/couler/)


## Environtment Variables

The following Pipeline workflow accesses various environment variables to 
set appropriate values in the configuration functions. 

> [**Note**]
> You can find a table of all relevant environment variables for 
> the Pipeline under `/home/jovyan/config/` in your JupyterHub filebrowser tab. Right click, Open With, and then select "Render Markdown" to see 
> a user-friendly table.

## Docker in the Remote Desktop

Docker is running in a sidecar to the remote desktop. 
What this means is that the daemon is not running inside 
the remote desktop directly, but adjacent to it.
This does not affect the way you interact with Docker, 
but is useful to know if you're looking for the daemon. 

To run Docker commands against the daemon, you can 
accomplish this using `sudo`, as follows:

```bash
sudo docker command ...
```

Where `command` is a Docker command such as `images`, `ps`, `build`, `push`, etc.

[Docker Commands](https://docs.docker.com/engine/reference/commandline/docker/)

## Using Docker and Pipelines

The below Sections will go over how to leverage Docker for your 
own Pipeline workflows. 

## Example Project

The following code shows a simple project folder:

```bash
project/
  dockerfile
  src/
      - requirements.txt
      - main.py        # main python app
      - lib/
          - helpers.py # subdirectory example
      - entrypoint.sh  # runs application via script
      - README.md
  README.md
```

The following is the contents of a `requirements.txt`:

```text
# /src/requirements.txt
xarray==2022.10.0
numpy==1.23.3
pandas==1.5.1
geopandas==0.12.1
```

Where the `entrypoint.sh` script looks something like this: 

```bash
#!/bin/bash

set -eu

python main.py
```

## Dockerfile

To build a Container Image, once the application has
been implemented, you will need to use a `dockerfile` manifest 
to allow the Container Engine to containerize your application. 

This `dockerfile` describes the environment which 
your application will run in. 
It it highly recommend to keep this as small (dependency disksize) as necessary to run your application. 

> __NOTE__  
> Smaller Images help speed up pulling of the Image as well as minimizing the 
> amount of resources required to host the application. Giving those 
> resources back to the application to use. 

```dockerfile
FROM python:3.10.9-slim

RUN apt update && apt install -y \
      software-properties-common

COPY src/ /app

RUN pip install -r /app/requirements.txt

# ENTRYPOINT ['/bin/bash', '-c', 'entryscript.sh'] # Custom script entrypoint
ENTRYPOINT ['python', 'main.py'] # Run Python application

```

## Building and Pushing to the Container Registry

First, to ensure we can push our built Container Image, we must login to our container registry with your provided username and password:

```bash
sudo docker login registry.eo4ph.geoanalytics.ca
```

Using the below information, replace the keywords in 
the subsequent Docker commands:

- `project-name` : The name of your project in the Container Registry
- `image-name` : The name that describes the Container Image
- `image-tag` : The version of the Image being built

Next, we must build and tag our Container Image 
so that it will push to the correct registry:

```bash
sudo docker build -t registry.eo4ph.geoanalytics.ca/project-name/image-name:image-tag .
```

Finally, we can push the successfully built Container Image:

```bash
sudo docker push registry.eo4ph.geoanalytics.ca/project-name/image-name:image-tag
```

You can also run your container: 

```bash
sudo docker run --rm -it registry.eo4ph.geoanalytics.ca/project-name/image-name:image-tag
```

## Using Your Docker Image in a Pipeline

The following two Python frameworks, 
[Hera-Workflows](https://hera-workflows.readthedocs.io/en/latest/index.html) 
and [Couler](https://couler-proj.github.io/couler/), can submit
Workflows to our pipeline executor and can be monitored here:
- [GEOAnalytics Pipelines](https://pipeline.eo4ph.geoanalytics.ca)

### Hera-Workflows

Hera-Workflows is a pipeline framework built by argoproj - the creators of Argo Workflows.
When you need more control over your pipeline, consider using 
Hera-Workflows SDK as it provides a more mature and stable framework. 

Hera-Workflows is capable of setting the number of 
parallel tasks permitted to run at any one time, this enables more 
fine-tuned allocation of resources. This especially comes in handy,
for example, when downloading data from Earthdata and server rate limits
causes a Failure for the task to complete. 

Caveats: 

- Argo requires unique names for Workflows and will complain if duplicates are submitted

Benefits:

-  Use both images from the private registry and and those publically available, too (eg. from DockerHub)

Thow following code will set up the framework for a simple pipeline:

```python
# Configure GEOAnalytics Pipeline Environment
ws = hera.workflow_service.WorkflowService(
    host=os.getenv('PIPELINE_HOST'),
    namespace=os.getenv('PIPELINE_NS'),
    token=os.getenv('PIPELINE_TOKEN')
)

node_selectors = {
  os.getenv('WORKFLOW_NODE_SELECTOR_KEY'):os.getenv('WORKFLOW_NODE_SELECTOR_VALUE_SMALL')
}

tolerations = [
        hera.toleration.Toleration(key=os.getenv('WORKFLOW_NODE_TOLERATION_KEY'), operator='Equal', effect='NoSchedule', value=os.getenv('WORKFLOW_NODE_TOLERATION_VALUE')),
        hera.toleration.Toleration(key='kubernetes.azure.com/scalesetpriority', operator='Exists', effect='NoSchedule')
    ]

w = hera.workflow.Workflow(
    name='unique-name-of-workflow',
    image_pull_secrets=[os.getenv('REGISTRY_PULL_SECRET')],
    node_selectors=node_selectors,
    tolerations=tolerations,
    service_account_name=os.getenv('WORKFLOW_SA'),
    parallelism=10, # Number of tasks to run in parallel 
)
```


Define your tasks and add them to the current workflow:

```python

def some_func():
  import os
  #do something
  print(os.getenv('TASKSAY'))

# Environment Variables for running source/container
env_list = [
  hera.env.Env(name='SOME_ENV', value='SOME_VAL'),
  hera.env.Env(name='TASKSAY', value='Workflows Are Powerful!')
]

# Task using a function as input
t1 = hera.task.Task(
  name='source-task',
  image='registry.eo4ph.geoanalytics.ca/project-name/image-name:image-tag',
  source=some_func,
  node_selectors=node_selectors,
  tolerations=tolerations,
  env=env_list
)

# Task using a prebuilt Docker Image running some Python application
t2 = hera.task.Task(
  name='container-task',
  image='registry.eo4ph.geoanalytics.ca/project-name/image-name:image-tag'
  command=['/bin/bash', '-c', 'python run.py'],
  node_selectors=node_selectors,
  tolerations=tolerations,
  env=env_list
)

w.add_task(t1) # Add source-task to workflow
w.add_task(t2) # Add container-task to workflow
t1 >> t2 # DAG - make t1 run before t2
```

Build and submit your workflow:

```python
ws.create_workflow(w.build())
```

### Couler 

Couler is a generic Pipeline framework that aims to interact with
all Pipeline engines - currently only Argo is supported.

```python
def pipeline_job(name, source, env_user):
    toleration = Toleration(os.getenv('WORKFLOW_NODE_TOLERATION_KEY'), 'NoSchedule', 'Exists')
    couler.add_toleration(toleration)
    toleration2 = Toleration('kubernetes.azure.com/scalesetpriority', 'NoSchedule', 'Exists')
    couler.add_toleration(toleration2)
    image_secret = ImagePullSecret(os.getenv('REGISTRY_PULL_SECRET'))
    couler.add_image_pull_secret(image_secret)
    return couler.run_script(
        image="registry.eo4ph.geoanalytics.ca/project-name/image-name:image-tag",
        step_name=name,
        source=source,
        env=env_user,
        node_selector={os.getenv('WORKFLOW_NODE_SELECTOR_KEY'):os.getenv('WORKFLOW_NODE_SELECTOR_VALUE')},
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
submitter = ArgoSubmitter(namespace=os.getenv('WORKFLOW_NS'))
deployment = couler.run(submitter=submitter)
deployment
```