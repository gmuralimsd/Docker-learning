# Docker Image Build Process (Step by Step)

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

# Step 1: Build the Docker Image

Run the following command:

```bash
docker build -t myapp:v1 .
```

### Breakdown

```bash
docker build
```

Builds a Docker image from the Dockerfile.

```bash
-t
```

Assigns a name (tag) to the image.

```text
myapp:v1
```

- Image Name: `myapp`
- Tag (Version): `v1`

```text
.
```

The dot (`.`) means the **current directory**.

This directory is called the **Build Context**.

Docker sends all files in this folder to the Docker daemon.

```text
Current Folder
      │
      ▼
Docker Daemon
```

---

# Step 2: Docker Reads the Dockerfile

Docker looks for a file named:

```text
Dockerfile
```

Project structure:

```text
myapp/
│
├── app.py
├── requirements.txt
└── Dockerfile
```

Docker reads the instructions **from top to bottom**.

---

# Step 3: Process the FROM Instruction

Docker reads:

```dockerfile
FROM python:3.11
```

### Meaning

Start the image using the official Python image.

Docker checks whether the image already exists locally.

```text
Do I already have python:3.11?
```

Check available images:

```bash
docker images
```

## Case 1: Image Exists

Docker uses the existing image.

```text
python:3.11
```

---

## Case 2: Image Does Not Exist

Docker downloads it from Docker Hub.

```text
Docker Hub
      │
      ▼
python:3.11
      │
      ▼
Local Machine
```

The first image layer is created.

```text
Layer 1
python:3.11
```

---

# Step 4: Process WORKDIR

Docker reads:

```dockerfile
WORKDIR /app
```

### Meaning

Create the directory:

```text
/app
```

inside the image if it does not already exist.

Docker changes the current working directory to:

```text
/app
```

Current layers:

```text
Layer 1
Python Base Image

↓

Layer 2
WORKDIR /app
```

---

# Step 5: Process COPY

Docker reads:

```dockerfile
COPY . .
```

### Meaning

Copy everything from your project directory into the current working directory inside the image.

Source:

```text
app.py

requirements.txt

Dockerfile
```

Destination:

```text
/app
```

Result:

```text
/app
│
├── app.py
├── requirements.txt
└── Dockerfile
```

Docker creates another layer.

```text
Layer 3

Copied Files
```

---

# Step 6: Process RUN

Docker reads:

```dockerfile
RUN pip install -r requirements.txt
```

Docker creates a temporary container.

```text
Temporary Container
```

Inside that container it runs:

```bash
pip install -r requirements.txt
```

Packages are downloaded.

```text
Internet
     │
     ▼
PyPI
     │
     ▼
Flask Package
     │
     ▼
Docker Image
```

After installation:

- Temporary container is deleted.
- Installed packages remain inside the image.

New layer:

```text
Layer 4

Installed Dependencies
```

---

# Step 7: Process EXPOSE

Docker reads:

```dockerfile
EXPOSE 5000
```

### Meaning

This does **NOT** open the port.

It simply documents that the application uses port **5000**.

```text
Image Metadata

Application Port

5000
```

---

# Step 8: Process CMD

Docker reads:

```dockerfile
CMD ["python","app.py"]
```

Docker stores this as the default command.

It is **NOT executed during image build**.

It runs only when a container starts.

```text
Container Starts

↓

python app.py
```

---

# Step 9: Docker Creates the Image

Docker combines all layers.

```text
Layer 5

CMD

↓

Layer 4

RUN pip install

↓

Layer 3

COPY Files

↓

Layer 2

WORKDIR

↓

Layer 1

Python Base Image
```

Final image:

```text
myapp:v1
```

Check available images:

```bash
docker images
```

Example:

```text
REPOSITORY   TAG

myapp        v1
```

---

# Step 10: Run the Image

Run:

```bash
docker run -d -p 5000:5000 myapp:v1
```

Docker performs the following:

```text
Image

↓

Create Writable Layer

↓

Container

↓

Execute CMD

↓

python app.py
```

Open your browser:

```text
http://localhost:5000
```

Output:

```text
Hello Docker!
```

---

# Internal Docker Build Flow

```text
docker build

       │
       ▼

Read Dockerfile

       │
       ▼

FROM

       │
       ▼

Base Image

       │
       ▼

WORKDIR

       │
       ▼

COPY

       │
       ▼

RUN

       │
       ▼

EXPOSE

       │
       ▼

CMD

       │
       ▼

Create Image

       │
       ▼

Store Image Locally
```

---

# Docker Layers

Each important Dockerfile instruction creates a layer.

```text
Docker Image

│
├── Layer 5 : CMD
├── Layer 4 : RUN
├── Layer 3 : COPY
├── Layer 2 : WORKDIR
└── Layer 1 : FROM
```

All image layers are **Read-Only**.

When a container starts, Docker adds one extra layer.

```text
Read-Only Layers

↓

Writable Container Layer
```

---

# Docker Build Cache

Suppose you modify only `app.py`.

Dockerfile:

```dockerfile
FROM python:3.11
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python","app.py"]
```

Docker checks every layer.

Result:

```text
Step 1 : FROM python:3.11
Using cache

Step 2 : WORKDIR /app
Using cache

Step 3 : COPY . .
Rebuilding

Step 4 : RUN pip install -r requirements.txt
Running Again

Step 5 : CMD
Done
```

---

# Best Practice for Faster Builds

Instead of:

```dockerfile
COPY . .

RUN pip install -r requirements.txt
```

Use:

```dockerfile
COPY requirements.txt .

RUN pip install -r requirements.txt

COPY . .
```

### Why?

If only your application code changes and `requirements.txt` stays the same:

- Docker reuses the cached dependency layer.
- `pip install` does not run again.
- Image builds much faster.

---

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

> **Key Point:** Docker builds an image one instruction at a time. Each major instruction creates a new layer. These layers are cached, making subsequent builds much faster when unchanged.
