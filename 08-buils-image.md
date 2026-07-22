# Docker Image Build Process

## Project Structure

Suppose you have the following project:

```text
myapp/
│
├── app.py
├── requirements.txt
└── Dockerfile
```

---

## app.py

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def home():
    return "Hello Docker!"

app.run(host="0.0.0.0", port=5000)
```

---

## requirements.txt

```text
flask
```

---

## Dockerfile

```dockerfile
FROM python:3.11

WORKDIR /app

COPY . .

RUN pip install -r requirements.txt

EXPOSE 5000

CMD ["python", "app.py"]
```

---

**Run the following command:**

```bash
docker build -t myapp:v1 .

```

### Breakdown

```bash
docker build --> Builds a Docker image from the Dockerfile.

-t --> Assigns a name (tag) to the image.

myapp:v1 --> Image Name: `myapp` --> Tag (Version): `v1`

The dot (`.`) means the **current directory**.

This directory is called the **Build Context**.

Docker sends all files in this folder to the Docker daemon.
```

---

**check created image**
```
docker images
```

*output*

myapp:v1

for upload a image to dockerhub we need to change image name as like as below 

**remane the image**

Syntax:
```
docker tag <LOCAL_IMAGE>  <DOCKERHUB_USERNAME/REPOSITORY>:<VERSION>

```
For example

```
docker tag myapp:v1 dockwithmurali/my-apps:v1
```

output 

dockwithmurali/my-apps:v1

**use this command to login into docker hub**

```
docker login
```

***use below command to push an image to dockerhub***
```
docker push dockwithmurali/my-apps:tagname
```

example 

docker push dockwithmurali/my-apps:v1

---

**export DH_USER="dockwithmurali"**

this command is used to export our docker hub reposotory 

useally DH_USER is stor the user name


# Complete Docker Image Build Flow

```text
Project Folder
      │
      ▼
docker build -t myapp:v1 .
      │
      ▼
Read Dockerfile
      │
      ▼
Download Base Image (If Needed)
      │
      ▼
Create FROM Layer
      │
      ▼
Create WORKDIR Layer
      │
      ▼
Create COPY Layer
      │
      ▼
Create RUN Layer
      │
      ▼
Store EXPOSE and CMD Metadata
      │
      ▼
Generate Docker Image
      │
      ▼
docker images
      │
      ▼
docker run myapp:v1
      │
      ▼
Container Starts
      │
      ▼
Runs:

python app.py
```

**export DH_USER="dockwithmurali"**

this command is used to export our docker hub reposotory 

useally DH_USER is stor the user name 
---

# Summary

| Dockerfile Instruction | Purpose |
|------------------------|---------|
| `FROM` | Select the base image |
| `WORKDIR` | Set the working directory |
| `COPY` | Copy files into the image |
| `RUN` | Execute commands during image build |
| `EXPOSE` | Document the application's listening port |
| `CMD` | Specify the default command when a container starts |
