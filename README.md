## How To

### Github -> AWS
- Apply terraform locally from ```terraform/oidc``` folder to create OIDC connection to your aws account
- Copy output variable to github secrets under the name ```AWS_ROLE_ARN```

### pre-commit

- install pre-commit following instructions https://pre-commit.com/#install

###

### Docker Build & Push locally
- run at root folder: \
```docker build -t <your-dockerhub-username>/mt-app:<desired_env> .``` \
```docker push <your-dockerhub-username>/mt-app:<desired_env>```

### Github Actions secrets
```AWS_ROLE_ARN``` Role previous created for OIDC \
```DOCKER_USERNAME``` User for Dockerhub \
```DOCKER_HUB_AUTH``` Password for Dockerhub


### Explaining how the code works, how to deploy the application and how to verify its successful deployment.

Application can be deployed locally with terraform
```
terraform init -backend-config=env/backend/<desired_env>.tfvars

terraform plan -var-file=env/<desired_env>.tfvars

terraform apply -var-file=env/<desired_env>.tfvars
```
To check it run
```
 aws eks --region <region> update-kubeconfig --name mytomorrows-<env>

kubectl get svc | grep my-app-service | kubectl get svc my-app-service | awk 'NR==2 {print "http://" $4}'
```


### Explain the decisions made during the design and implementation of the solution.
Solution was implemented using EKS and Helm to deploy application to a cluster, variables for helm are managed at terraform


### Explain the networking strategy you would adopt to deploy production ready applications on AWS.
Current security is restricted at AWS security groups

### Describe how you would implement a solution to grant access to various AWS services to the deployed application.
Permission can be grated from an OIDC credential create for the eks cluster

### Describe how would you automate deploying the solution across multiple environments using CI/CD.
this was partially implemented but i am having an issue with auth from github to the deployment of helm

### Discuss any trade-offs considered when designing the solution.
this solution take into account multiple environments but isn't optimised for blue/green deployment

### Explain how scalability, availability, security, and fault tolerance are addressed in the solution.
scalability, availability, security, and fault tolerance are addressed with number of service running on the cluster. Security is addressed with ec2 resources

### Suggest any potential enhancements that could be made to improve the overall solution.
- Security controls at EKS level
- Health Check
- DNS Entry
- Require CI/CD steps to approve merge
