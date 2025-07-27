+++
title = 'How to get hands-on in Cloud'
date = 2024-02-11

+++

### Introduction

- So many people struggle with where to get started with AWS and cloud technologies in general. There is popular "How do I learn to be a Linux admin?" post that inspired me to write an equivalent for cloud technologies. This post serves as a guide of goals to grow from basic AWS knowledge to understanding and deploying complex architectures in an automated way. Feel free to pick up where you feel relevant based on prior experience.

### Assumptions:

- You have basic-to-moderate Linux systems administration skills
- You are at least familiar with programming/scripting. You don't need to be a whiz but you should have some decent hands-on experience automating and programming.
- You are willing to dedicate the time to overcome complex issues.
- You have an AWS Account and a marginal amount of money to spend improving your skills.

### How to use this guide:

- This is not a step-by-step how-to guide.
- You should take each goal and "figure it out". I have hints to guide you in the right direction.
- Google is your friend. AWS Documentation is your friend. Stack Overflow is your friend.
- Find out and implement the "right way", not the quick way. Ok, maybe do the quick way first then refactor to the right way before moving on.
- Shut down or de-provision as much as you can between learning sessions. You should be able to do everything in this guide for literally less than $50 using the AWS Free Tier. Rebuilding often will reinforce concepts anyway.
- Skip ahead and read the Cost Analysis and Automation sections and have them in the back of your mind as you work through the goals.
- Lastly, just get hands on, no better time to start then NOW.

### Project Overview

- This is NOT a guide on how to develop websites on AWS. This uses a website as an excuse to use all the technologies AWS puts at your fingertips. The concepts you will learn going through these exercises apply all over AWS.
- This guide takes you through a maturity process from the most basic webpage to an extremely cheap scalable web application. The small app you will build does not matter. It can do anything you want, just keep it simple.
- Need an idea? Here: Fortune-of-the-Day - Display a random fortune each page load, have a box at the bottom and a submit button to add a new fortune to the random fortune list.

### Account Basics

- Create an IAM user for your personal use.
- Set up MFA for your root user, turn off all root user API keys.
- Set up Billing Alerts for anything over a few dollars.
- Configure the AWS CLI for your user using API credentials.
- **Checkpoint: You can use the AWS CLI to interrogate information about your AWS account.**

### Web Hosting Basics

- Deploy a EC2 VM and host a simple static "Fortune-of-the-Day Coming Soon" web page.
- Take a snapshot of your VM, delete the VM, and deploy a new one from the snapshot. Basically disk backup + disk restore.
- **Checkpoint: You can view a simple HTML page served from your EC2 instance.**

### Auto Scaling

- Create an AMI from that VM and put it in an autoscaling group so one VM always exists.
- Put a Elastic Load Balancer infront of that VM and load balance between two Availability Zones (one EC2 in each AZ).
- **Checkpoint: You can view a simple HTML page served from both of your EC2 instances. You can turn one off and your website is still accessible.**

### External Data

- Create a DynamoDB table and experiment with loading and retrieving data manually, then do the same via a script on your local machine.
- Refactor your static page into your Fortune-of-the-Day website (Node, PHP, Python, whatever) which reads/updates a list of fortunes in the AWS DynamoDB table. (Hint: EC2 Instance Role)
- **Checkpoint: Your HA/AutoScaled website can now load/save data to a database between users and sessions**

### Web Hosting Platform-as-a-Service

- Retire that simple website and re-deploy it on Elastic Beanstalk.
- Create a S3 Static Website Bucket, upload some sample static pages/files/images. Add those assets to your Elastic Beanstalk website.
- Register a domain (or re-use and existing one). Set Route53 as the Nameservers and use Route53 for DNS. Make www.yourdomain.com go to your Elastic Beanstalk. Make static.yourdomain.com serve data from the S3 bucket.
- Enable SSL for your Static S3 Website. This isn't exactly trivial. (Hint: CloudFront + ACM)
- Enable SSL for your Elastic Beanstalk Website.
- **Checkpoint: Your HA/AutoScaled website now serves all data over HTTPS. The same as before, except you don't have to manage the servers, web server software, website deployment, or the load balancer.**

### Microservices

- Refactor your EB website into ONLY providing an API. It should only have a POST/GET to update/retrieve that specific data from DynamoDB. Bonus: Make it a simple REST API. Get rid of www.yourdomain.com and serve this EB as api.yourdomain.com
- Move most of the UI piece of your EB website into your Static S3 Website and use Javascript/whatever to retrieve the data from your api.yourdomain.com URL on page load. Send data to the EB URL to have it update the DynamoDB. Get rid of static.yourdomain.com and change your S3 bucket to serve from www.yourdomain.com.
- **Checkpoint: Your EB deployment is now only a structured way to retrieve data from your database. All of your UI and application logic is served from the S3 Bucket (via CloudFront). You can support many more users since you're no longer using expensive servers to serve your website's static data.**

### Serverless

- Write a AWS Lambda function to email you a list of all of the Fortunes in the DynamoDB table every night. Implement Least Privilege security for the Lambda Role. (Hint: Lambda using Python 3, Boto3, Amazon SES, scheduled with CloudWatch)
- Refactor the above app into a Serverless app. This is where it get's a little more abstract and you'll have to do a lot of research, experimentation on your own.
  - The architecture: Static S3 Website Front-End calls API Gateway which executes a Lambda Function which reads/updates data in the DyanmoDB table.
- Use your SSL enabled bucket as the primary domain landing page with static content.
- Create an AWS API Gateway, use it to forward HTTP requests to an AWS Lambda function that queries the same data from DynamoDB as your EB Microservice.
- Your S3 static content should make Javascript calls to the API Gateway and then update the page with the retrieved data.
- Once you have the "Get Fortune" API Gateway + Lambda working, do the "New Fortune" API.
- **Checkpoint: Your API Gateway and S3 Bucket are fronted by CloudFront with SSL. You have no EC2 instances deployed. All work is done by AWS services and billed as consumed.**

### Cost Analysis

- Explore the AWS pricing models and see how pricing is structured for the services you've used.
- Answer the following for each of the main architectures you built:
  - Roughly how much would this have costed for a month?
  - How would I scale this architecture and how would my costs change?
- Architectures
  - Basic Web Hosting: HA EC2 Instances Serving Static Web Page behind ELB
  - Microservices: Elastic Beanstalk SSL Website for only API + S3 Static Website for all static content + DynamoDB Table + Route53 + CloudFront SSL
  - Serverless: Serverless Website using API Gateway + Lambda Functions + DynamoDB + Route53 + CloudFront SSL + S3 Static Website for all static content

### Automation !!! This is REALLY important !!!

- These technologies are the most powerful when they're automated. You can make a Development environment in minutes and experiment and throw it away without a thought. This stuff isn't easy, but it's where the really skilled people excel.
- Automate the deployment of the architectures above. Use whatever tool you want. The popular ones are AWS CloudFormation or Teraform. Store your code in AWS CodeCommit or on GitHub. Yes, you can automate the deployment of ALL of the above with native AWS tools.
- I suggest when you get each app-related section of the done by hand you go back and automate the provisioning of the infrastructure. For example, automate the provisioning of your EC2 instance. Automate the creation of your S3 Bucket with Static Website Hosting enabled, etc. This is not easy, but it is very rewarding when you see it work.

### Continuous Delivery

- As you become more familiar with Automating deployments you should explore and implement a Continuous Delivery pipeline.
- Develop a CI/CD pipeline to automatically update a dev deployment of your infrastructure when new code is published, and then build a workflow to update the production version if approved. Travis CI is a decent SaaS tool, Jenkins has a huge following too, if you want to stick with AWS-specific technologies you'll be looking at CodePipeline.

### Miscellaneous / Bonus

- These didn't fit in nicely anywhere but are important AWS topics you should also explore:
- IAM: You should really learn how to create complex IAM Policies. You would have had to do basic roles+policies for the EC2 Instance Role and Lambda Execution Role, but there are many advanced features.
- Networking: Create a new VPC from scratch with multiple subnets (you'll learn a LOT of networking concepts), once that is working create another VPC and peer them together. Get a VM in each subnet to talk to eachother using only their private IP addresses.
- KMS: Go back and redo the early EC2 instance goals but enable encryption on the disk volumes. Learn how to encrypt an AMI.

### Final Thoughts

- I've been recently recruiting for Cloud Systems Engineers and Cloud Systems Administrators. We've interviewed over a dozen local people with relevant resume experience. Every single person we interviewed would probably struggle starting with the DynamoDB/AutoScaling work. I'm finding there are very few people that HAVE ACTUALLY DONE THIS STUFF. Many people are familiar with the concepts, but when pushed for details they don't have answers or admit to just peripheral knowledge. You learn SO MUCH by doing.
- If you can't find an excuse or get support to do this as part of your job I would find a small but flashy/impressive personal project that you can build and show off as proof of your skills. Open source it on GitHub, make professional documentation, comment as much as is reasonable, and host a demo of the website. Add links to your LinkedIn, reference it on your resume, work it into interview answers, etc. When in a job interview you'll be able to answer all kinds of real-world questions because you've been-there-done-that with most of AWS' major services.

### Learning DevOps

- Well, the best way to learn is to actually do something. Create a small service in your favourite framework. Something really small, an echo service will probably suffice. Then stop "developing" and switch on "Ops mode".
  - Automate:
    - builds (build & packaging scripts)
    - deployments - try all 3 major approaches:
      - push deployment: running a command on a central server that orchestrates everything (Ansible, Salt, chef-solo, ...)
      - pull deployment: agents running on your target server, that pull the latest changes (Chef, Puppet, ...)
      - immutable infrastructure: VMs or containers that are never modified, only recreated (CloudFormation, Docker, ...)
  - include database updates in your deployment orchestration and possibly include even environment pre-warming/pre-caching
- functional tests, especially fast smoke tests
- Add:
- high availability/load balancing (Nginx, HAProxy, Apache, Elastic Load Balancer)
- detailed technical monitoring and graphing (Nagios, Zabbix, Cloudwatch)
- availability monitoring (Pingdom)
- a status page (can't give you a decent example; you can build your own, but host it somewhere else than your main "app")
- log collection and shipping (Splunk, Graylog)
- Basically, for almost everything I listed google options and pick an "Ops stack". Then implement that as best you can.
- Oh, and by the way, while working on the "Ops stack", only "develop" things in support of this Ops work in your "app".
- security (you should already know a great deal from your work as a developer, the general things such as no open ports, no plain text passwords, etc.)
- related to security: secret management (Chef, Puppet - I think, Vault)
- backups, backups, backups: for the database and basically anything which is not reproducible from source (which in general should only be your database data)

#### User 1

- Set up & configure Jenkins to pull and build. Set up and configure nginx, MySQL/PostgreSQL, init files for your app, iptables to firewall off unnecessary ports, DNS, and LetsEncrypt SSL certs (and the associated nginx configs). Make sure nothing that doesn't need to be is running as root. Make sure that what is running needs to be running, and nothing else.
- Then, start automating it. Toss in cloud features like ELBs, RDS instances, cloudformation (or terraform, etc), autoscaling groups, autoscaling container services, etc

#### User 2

- There's two major types of monitoring you should be concerned with, log aggregation and metrics collection.
- For log aggregation the most mature open-source solution I know of is the ELK stack (Elasticsearch for storage, Logstash for aggregation, Kibana for visualization). Set that up (bonus points if you use Puppet/Chef/Ansible/Whatever to set it up for you), and make sure you can detect anomalies from your visualization interface.
  - I'll also throw my hat in for setting up (and securing, without Shield) an ELK stack here
  - recently set up the Elastic "stack" with fluentd shipping logs from Kubernetes nodes
- For metrics collection, Prometheus has a special place in my heart, I love its architecture. That said there are quite a few options, InfluxDB is another (arguably simpler) option. You can then use something like Grafana to visualize the metrics.
- For configuration management tools, Chef is complicated and you better know Ruby and have a few months to dig into it. Puppet is simpler, but has a lot of weird quirks. Ansible is simple, but not as powerful (or used); so pick your poison, they all are pretty similar conceptually.
- Basically at the end of your journey you should be able to kill any server (or network connection, or application running on a server) and be alerted to it in near realtime. You should be able to diagnose what's wrong quickly using Kibana and Grafana, and finally you should be able to destroy and recreate the server using a few well understood commands and have it be completely reconfigured and ready to accept connections. Of course this is pie in the sky (like having perfect unit/integration testing for a very large application), but that should be the goal.

#### User 3

- Have you written a webapp as a side project before? If yes, then great:
  1. Find a bare VM provider, e.g. Linode or Digital Ocean or EC2.
  2. Figure out a way to get your code up there, even if it's ghetto. e.g. git pull from github. Get the app server running.
  3. Figure out how to expose your app to the internet. Buy a domain name, get it to point to your IP addr. Soon enough you will realize that DNS load balancing is terrible...
  4. Then you should install Nginx or HA proxy and put your app behind it. Run your app on localhost, only nginx should be exposed.
  5. Once everything is up, iptables is your next concern, expose only ports you want and disable everything else.
  6. Repeat 1-5 on a second instance.
  7. Soon enough you will find that repeating the same thing sucks, so you will write Python script using Fabric library. But then you realize someone else have done this already, after a quick googling, you will find Ansible or Salt. Use those instead.
  8. Rinse and repeat for other networked things. e.g. databases, mail relays, cache servers, etc.
  9. Evolve your approaches, rewrite ghetto stuff, make your artifact as immutable as possible with very few network dependencies...
- That's pretty much the entire devops evolution up until 3 years ago. Once you get good in these...
  1. Start reading about Linux container and why they are useful.
  2. Install docker and try to get your toy project in a dockerfile.
  3. Repeat the learning exercise again on deploying Docker containers.

#### User 4

- I would look at these topics and in this order:
  1. Containerization - can you build a hello world web app in any language then Dockerize it.
  2. Now break it into two containers - one is the original hello world but now it calls an API on a 2nd container that responds with hello world in one of 10 different languages. Just hard code this the point is that it’s now 2 containers for your “app”.
  3. Create a Docker repository in GCP’s Artifact Registry and upload your images. Now remove them from your local Docker registry and run them again but this time pulling them from your registry on GCP.
  4. Use Cloudbuild to build them.
  5. Spin up a local Kubernetes cluster such as Minikube.
  6. Read docos about K8s deployments and service types. Special attention to ClusterService, NodePort, LoadBalancer.
  7. Deploy the first version of your hello world. Maybe try to point your YAML to the local registry then the one on GCP you created.
  8. Create a service to access your app.
  9. Figure out how to turn in and access (via proxy) the kubernetes dashboard.
  10. Now deploy the 2nd version of your app. Learn a bit more about K8s networking, pod horizontal scaling, pod resource claims, kill some pods, etc.
  11. Learn Skaffold.
  12. Create GKE cluster.
  13. Deploy same app.
  14. Learn about K8s Ingress.
  15. Get familiar with GKEs console.
  16. Use knowledge of Skaffold to understand and use the brand new Google Cloud Deploy.
