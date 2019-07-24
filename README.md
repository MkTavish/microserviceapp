# **Node.js Microservices Deployed on EC2 Container Service**

## Deployment

1. Create an AWS free tier account
2. Install Docker to build the image files that you will run in your containers.
3. Install the AWS CLI to push images to Amazon Elastic Container Registry.
   -Test your AWS CLI with `aws s3 ls` this should list your available s3 buckets
   -Run `aws configure` to configure your credentials.
4. Clone this repo `git clone https://github.com/MkTavish/microserviceapp.git`.
5. Navigate to the Amazon Elastic Container Registry (Amazon ECR) and create a repository.
6. Authenticate Docker login with  ws ecr get-login --no-include-email --region [region]. 
   Paste the output in the console to login.
7. Build the image in the terminal with `docker build -t imagename .`, then tag the image `docker tag imagename:v1 [account-id].dkr.ecr.[region].amazonaws.com/imagename:v1`.
8. Push your image to ECR `docker push [account-id].dkr.ecr.[region].amazonaws.com/imagename:v1`.
9. Launch an ECS cluster using the Cloudformation template:
   From your CLI, navigate to your infrastructure directory then run
   ```
   $ aws cloudformation deploy \
   --template-file ecs.yml \
   --region enterregion \
   --stack-name enterstackname \
   --capabilities CAPABILITY_NAMED_IAM
   ```
10. Navigate to ECS and create a Task Definition, specify a container name and specify your ECR repo image.
    Specify Target Group and Listner.
11. Create a service and deploy your monolith into a cluster. Confirm its running from your Loadbalancer URL.
12. Break the monolith into microservices. 
    - Create 3 repositories and name them.
    - Build and push the images as described in 6 - 8.
13. Create a service for your containers as described above. You can add 3 containers to a single service.
14. Configure Target Groups and Listners for your services.
15. Forward traffic to your created microservices form your Loadbalancer.
16. Validate your services via your loadbalancer with the http address ending with the service name. `http://[DNS name]/api/microservicename`
