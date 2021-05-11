# CI/CD With cloudbuild 

If the ```gcloud config get-value project``` command does not return the ID of the project you selected, configure Cloud Shell to use your project.
gcloud config set project [PROJECT_ID]


Enable the required APIs.
```shell
gcloud services enable container.googleapis.com \
    cloudbuild.googleapis.com \
    sourcerepo.googleapis.com \
    containeranalysis.googleapis.com
```

Create a GKE cluster that you will use to deploy the sample application.

```shell
gcloud container clusters create hello-cloudbuild \
    --num-nodes 1 --zone us-central1-b
```

Use Git, configure it with your name and email address. Git will use those to identify you as the author of the commits you will create in Cloud Shell.

```shell
git config --global user.email "[YOUR_EMAIL_ADDRESS]"
git config --global user.name "[YOUR_NAME]"
```

Run the gcloud credential helper. This will connect your Google Cloud user account to Cloud Source Repositories.

```shell
git config --global credential.helper gcloud.sh
```

## Creating the Git repositories in Cloud Source Repositories

Create the two Git repositories (app and env), and initialize the app one with some sample code.
Create the two Git repositories.

```shell
gcloud source repos create hello-cloudbuild-app
gcloud source repos create hello-cloudbuild-env
```

## Clone the sample code from GitHub.

```
cd ~
git clone https://github.com/GoogleCloudPlatform/gke-gitops-tutorial-cloudbuild \
    hello-cloudbuild-app
```

## Configure Cloud Source Repositories as a remote.

```
cd ~/hello-cloudbuild-app
PROJECT_ID=$(gcloud config get-value project)
git remote add google \
    "https://source.developers.google.com/p/${PROJECT_ID}/r/hello-cloudbuild-app"
```
*app.py* file
```
from flask import Flask
app = Flask('hello-cloudbuild')

@app.route('/')
def hello():
  return "Hello World!\n"

if __name__ == '__main__':
  app.run(host = '0.0.0.0', port = 8080)
  ```
  
  
