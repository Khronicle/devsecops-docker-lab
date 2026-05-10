# devsecops-docker-lab

A simple, secure Flask web application demonstrating best practices for container development and deployment. This project showcases Docker security, health checks, environment configuration, and containerization of Python web applications.


## Project Structure

```
devsecops-labs/
├── app.py                 # Flask application with routes and configuration
├── requirements.txt       # Python package dependencies (Flask 3.0.0)
├── Dockerfile             # Multi-stage secure Docker configuration
├── README.md              # Project documentation (this file)
├── images                 # Project output images.
├── templates/
│   └── index.html         # Jinja2 HTML template for the home page
└── myenv/                 # Python virtual environment (development only)
    ├── Scripts/           # Activation scripts
    └── Lib/
        └── site-packages/ # Installed Python packages
```

 
## Phase 0 - Pull and Inspect a Public Container

### commands run: 
```
1. docker pull nginx:alpine
2. docker run -d -p 8080:80 --name my-nginx nginx:alpine
3. docker ps
4. docker logs my-nginx
5. docker rm -f my-nginx
```
### Output: 

![alt text](/images/devsecops-lab-phase-0.png)


![alt text](/images/dscl-phase-0-nginx-welcome-screen-1.png)


![alt text](/images/dscl-phase-0-container%20removal.png)


## Phase 1 - Build, Push, Pull & Share Your Own Image

Under this phase the following actions were performed: 
1. A flask web application called app.py was created with it's accompanying html structure called index.html.
2. A requirements file was added to the project
3. A secure docker file was setup
4. Added a docker ignore files to exclude some files from the image yet to be created. 

After doing all that, the following commands were run to create an image locally: 

### Commands: 

```
1. docker build -t my-web-app .
2. docker run -d -p 5000:5000 --name webapp-test my-web-app
```

### Output: 

![alt text](/images/dscl-phase-1-container-setup-issues.png)

![alt text](/images/dscl-phase-1-container-setup-fix.png)

![alt text](/images/dscl-phase-1-container-setup-and-run-1.png)


#### Build logs: 

![alt text](/images/dscl-phase-1-image-buildlogs-1.png)

![alt text](/images/dscl-phase-1-image-buildlogs-2.png)

![alt text](/images/dscl-phase-1-image-buildlogs-3.png)

![alt text](/images/dscl-phase-1-image-buildlogs-4.png)

![alt text](/images/dscl-phase-1-image-buildlogs-final.png)


#### Running the web local container web application and health check

![alt text](/images/dscl-phase-1-local-container-web-app.png)



![alt text](/images/dscl-phase-1-local-container-web-app-health.png)


### Pushing to Docker Hub: 

Under this section there was the need to login into docker hub from the terminal using the username and api key generated. Once done, the image was ready to be tagged and pushed to docker hub. The following commands were used to achieve this: 

### Commands: 

```
1. docker login -u khronicles1
2. docker tag my-web-app khronicles1/my-web-app:v1.0.0-john
3. docker push khronicles1/my-web-app:v1.0.0-john
```

### Output: 

![alt text](/images/dscl-phase-1-image-tagging-and-deployment.png)


### Pulling pushed docker image as a new user: 

After pushing to the docker hub, there was the need to act as a new user by deleting the local image and then pulling the image pushed to the docker hub and run it. The following commmands was used to achieve this: 

#### Commands: 

```
1. docker rmi khronicles1/my-web-app:v1.0.0-john
2. docker pull khronicles1/my-web-app:v1.0.0-john
3. docker run -d -p 5000:5000 --name khronicles1/my-web-app:v1.0.0-john
```

#### Output: 

![alt text](/images/dscl-phase-1-pulled-image-and-container-setup.png)



## Phase 2: Automate with a DevSecOps Pipeline (GitHub Actions)

After successfully completing the setup for the docker image, pushing it to docker hub and pulling it back locally, the next phase required that the entire project's workflow must be setup to ensure that whenever we push to git, the CI/CD pipeline will execute and trivy will scan for any vulnerabilities. The following actions were done: 

1. A github action workflow yaml file was setup.
2. The actions file was pushed to the repo to ensure the CI/CD pipeline gets executed

### Output: 

![alt text](/images/dscl-phase-2-git-commit.png)


![alt text](/images/dscl-phase-2-git-push-error.png)


![alt text](/images/dscl-phase-2-git-push-fix.png)
