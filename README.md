# Guestbook Application – Source Code Repository

This repository contains the **application source code** for the Guestbook project.
It is used for **building container images** and running **Continuous Integration (CI)**.

The application is deployed using GitOps in a separate repository managed by Argo CD.

Deployment repository:
https://github.com/ranper85/rani-guestbook-app-config


## Purpose of This Repository

The purpose of this repository is to manage the **application layer** only.

It is responsible for:
- Frontend and backend source code
- Container image definitions (Containerfiles)
- CI pipelines that build and publish images

This repository **does not contain deployment configuration** and does not deploy
anything directly to the cluster.


## Repository Structure
```text
.
├── frontend/
│   ├── index.html
│   ├── nginx configuration
│   └── Containerfile
│
├── backend/
│   ├── application source code
│   └── Containerfile
│
└── .github/
    └── workflows/
        └── CI pipelines


```
---
## Application Components
    
### Frontend
The frontend provides the web interface where users can view and submit  
guestbook entries.  
It is served using **Nginx** and forwards API requests to the backend using  
the `/api/...` path.

### Backend
The backend provides a REST API for handling guestbook entries.  
It processes requests from the frontend and communicates with  
PostgreSQL and Redis inside the cluster.

---
## Container Images           

The frontend and backend are built as **separate container images**.

- Images are built automatically using **GitHub Actions**
- Images are pushed to **Quay.io**
- Each build produces a **unique and immutable image tag**
  based on the GitHub Actions run number and commit SHA

Example image tag:1.0.25-a1b2c3d

A `latest` tag is also published for reference.

---
## CI Workflow                   

1. A change is pushed to the `main` branch
2. GitHub Actions is triggered automatically
3. New container images are built
4. Images are pushed to the container registry
5. The deployment repository is updated with the new image tags

After this step, Argo CD handles the deployment automatically.




