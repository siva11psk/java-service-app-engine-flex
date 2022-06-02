# Deploying a java service to GCP APP Engine  
  
Project created for demonstrating a java service deployment to APP Engine.  
  
## Prerequisites  
  
* A GCP project with App Engine application created, follow this [documentation](https://github.com/siva11psk/gcp-app-engine-terraform).    
* A Service account with the required IAM roles and a service account key json file for authentication, follow this [documentation](https://github.com/siva11psk/gcp-app-engine-terraform).  
* Maven and Java installed.  
* Gcloud CLI installed.  
  
## Deployment steps  

### From local environment  

1. Checkout the repo.  
2. Take a look at the configuration in the app.yaml file and modify as needed.    
3. Run the maven build  
    `#maven build - creates the jar file in the ./target directory`  
    `mvn clean package`
4. Copy the artifacts to a staging directory and switch to it  
    `#create an artifacts directory and copy the jar file and app.yaml file into it`  
    `mkdir artifacts`  
    `cp ./target/*.jar ./artifacts`  
    `cp ./app.yaml ./artifacts`  
    `#deploy the application to app engine using gcloud cli`  
    `cd artifacts`  
5. Login to GCP  
    `gcloud auth login`
6. Select the GCP Project  
    `gcloud config set project {GCP_PROJECT_ID}`
7. Deploy the service to app engine  
    `gcloud app deploy`  
8. The `gcloud app deploy` command will output the url of the app engine service that got deployed.    
  
  
### From GitLab-CI  
  
1. Check-in the code with the pipeline yaml file to your repo in gitlab. The initial pipeline run may fail as the variables are not set yet.    
2. Configure the [pipeline variables](https://docs.gitlab.com/ee/ci/variables/#add-a-cicd-variable-to-a-project) as described below    
   1. **Key** - GOOGLE_APPLICATION_CREDENTIALS  
      **Value** - Contents of the service account json file   
      **Type** - File  
   2. **Key** - GCP_PROJECT_ID   
      **Value** - Your GCP project id     
      **Type** - Variable   
3. Run the pipeline.    
4. The logs from the `gcloud app deploy` command in the pipeline should output the url of the app engine service that got deployed.


## References

* [GAE Flex - Java](https://cloud.google.com/appengine/docs/flexible/java)
* [App.yaml](https://cloud.google.com/appengine/docs/flexible/java/configuring-your-app-with-app-yaml)
* [App Engine Gcloud commands](https://cloud.google.com/appengine/docs/flexible/java/configuring-your-app-with-app-yaml)