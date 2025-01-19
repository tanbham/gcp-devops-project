# CI/CD using GCP Cloud builder  

This project involves deploying a Python Flask application to a Google Kubernetes Engine (GKE) cluster using Continuous Integration/Continuous Deployment (CI/CD) with Google Cloud Build. The goal is to automate the deployment process, ensuring seamless integration of the Flask application with the GKE infrastructure

![image](https://github.com/user-attachments/assets/fe012795-11aa-4124-a222-68ad283ea83f)  

**Step1:** Write a simple pythonflask application : Refer app.py file  

**Step2:** Containerized the python application with Docker : Refer Dockerfile

**Step3:** Intergating github with cloud build :
- Enable Cloud build API in GCP project
-  Select region if needed or keep it as global only

![image](https://github.com/user-attachments/assets/d3d84dce-29aa-479f-8782-b4ecfc64e0cf)

-  Triggers --> This section manages Repositories [We see the list of repos managed by cloud build]

![image](https://github.com/user-attachments/assets/ac9a312d-b69a-4309-a212-267985f179f2)

![image](https://github.com/user-attachments/assets/4e3789b1-572b-4e37-a38a-4d066fc588e3)  

- Once source is selected as githib and if you are already logged in to github the authentication will take place automatically.
- At first, it will throw an error saying install google cloud build in github repos.
- Click on the same and we will get routed to github, install the google cloud build.

![image](https://github.com/user-attachments/assets/40b27cc3-2351-421f-9cee-58e56a8a842e)

![image](https://github.com/user-attachments/assets/72d91e39-9313-431f-8bf0-a786db155a8b)

![image](https://github.com/user-attachments/assets/7afabd75-c52b-4118-bdae-77aa5e36201e)

**Step4:**  Configure Trigger in cloud build  
This trigger describes when to execute the CI/CD pipeline, the pipeline code is written in an yaml file.  
- Event : Describes when to trigger the pipeline [on push to main branch]
- Source : Source Repo
- Branch : main

![image](https://github.com/user-attachments/assets/e59de243-9627-48b1-8a52-57ebf93dc838)

![image](https://github.com/user-attachments/assets/3acce75c-a48f-43b8-9b3a-7e2a98d49ea2)

- We selected to read the configuration from cloudbuild.yaml file in github repo.
- We need to write on yaml file and commit to github repo, cloud build will read the file accordingly.
- This yaml file will have CI or CD code

Step6: Write cloudbuild.yaml file : Refer cloudbuild.yaml

Conclusion:
- Whenever there is change in code, developer will create a feature branch from  main branch
- Do the required changes.
- Create a MR to merge feature to main branch
- Upon a successful commit to the main branch, cloud build trigger will get activate

 ```bash
steps:
#Step for docker build [CI]
- name: "gcr.io/cloud-builders/docker"
  args: ["build", "-t", "gcr.io./$PROJECT_ID/gcpdevops", "."]
#Step for docker push [CI]
- name: "gcr.io/cloud-builders/docker"
  args: ["push", "gcr.io./$PROJECT_ID/gcpdevops"]
#Step for deploying [CD]
- name: "gcr.io/cloud-builders/gke-deploy"
  args:
  - run
  - --filename=deployment.yaml
  - --image=gcr.io./$PROJECT_ID/gcpdevops
  - --location=us-central1-c
  - --cluster=devops-cluster
  - --namespace=mynamespace
```







