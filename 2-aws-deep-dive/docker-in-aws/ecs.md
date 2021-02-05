# ECS: Elastic Container Service

## ECS Clusters

- ECS Clusters are logical grouping of ECS instances
- EC2 instances run the ECS agent, which is basically a Docker container
- The ECS agent is used to register the EC2 instance with the ECS cluster
- ECS nodes use EC2 instances which run a special AMI (specialized Amazon Linux) built for ECS

## Task Definition

- A task definition is a JSON file which is used to tell ECS how to run a Docker container
- It can container information about:
    - Image Name
    - Port binding for container and host
    - Memory and CPU
    - Environment variables
    - Networking information
- Tasks can have independent IAM roles!

## ECS Service

- ECS Services help define how many tasks should run and how they should be run
- They ensure that the number of tasks desired is running across our fleet of EC2 instances (orchestration)
- An ECS service can be linked to a load balancer
- ECS Service types:
    - Replica: general Docker container, ECS will try to run as many as we tell him to run
    - Daemon: number if tasks is automatic, ECS will try to run one task per EC2 instance
- Deployment types for services:
    - Rolling updates
    - Blue/Green deployment using AWS CodeDeploy
- ECS Service with Application Load Balancer
    - When we don't specify a host port in the task definition, a random one is provided
    - Dynamic port forwarding: ALB will route the traffic to the assigned random port and will distribute the traffic between the running ECS services
    - Load balancers can not be added to existing ECS stacks. In order to add a load balancer we should create a new stack
    - The security group used by the EC2 instances should be modified to allow all traffic from the ALB
