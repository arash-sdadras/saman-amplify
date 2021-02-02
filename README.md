# saman-amplify

This project is a showcase of AWS Amplify to build and deploy a simple webpage with react and an AWS Lambda backend function written in Python.

## Overview

The web app consists of two components:

  - A static webpage, integrated with React.js
  - A callback function, which returns some simple data.

You need to upload the static webpage somewhere web accessible. There's a clearly marked value in the source of the webpage that needs to be changed to the endpoint of the callback. Once this is done, your static page will call the callback, and you'll see the output.


## Detail

The files you'll need to upload to make the static site work are:

  - `index.html`
  - `event_caller.js`

The code you'll need to host somewhere so that it can execute is:

  - `callbackcode.py`

The callback function is a Python Lambda function that is triggered through the API gateway whenever the page is refreshed and REST call is made to teh API Gateway.

# Architectural Considerations

Some of the architectural considerations:
- All communications through TLS
- Use of CDN to improve the site scalability and improve the site performance
- Configration of WAF to apply a number of security policies including OWASPP, etc.


# Architectural Constraints & Technical Debts:

1- Considerations on creation of private VPCs

Considering that the web site and the APIs are fronted by Cloudfront and WAF, at this point, I avoided the extra complexity of creating and managing the private VPCs for the static pages and the Lambda function. Since Lambda function does not need access to any backend resources, there is limited value. 

The static page can however be looked at later.

2- IAM roles have been created and applied for all resources. There is room to harden the assignment of the roles to the service account.

3- Expose the APIs under the custom domain name (samanapi.sgsys.com.au)


## Other Architectural options

While the architectural choices made (Lamda, Static site created by Amplify) delivered:
- Lowest change to the source code
- Reduced development and configuration effort 

There are a few other architectural options to achieve similar outcome.


## Architectural options # 1 - AWS Elastic Beanstalk

The Beanstalk takes care of setting up majority of the components on AWS.

In this architecture, the Python code (required changes) and the static page will be installed on EC2 servers fronted by load balancers. 
This option will be costly (average).


## Architectural options # 2 - A container on K8 (EKs)

The Python callback code (with required change) and static page will be set up as a container, managed on EKS for better scalability and portability. 
In this case, you will have the cost of Cluster and the nodes (around $4 per day) - high.


## Architectural options # 3 - Serverless Fargate on K8 (EKs)

The Python callback code will be set up as a function on fargate on EKS for better scalability, interoperability and ease of maintenance and management.
The cost will be average since the cluster still needs to be up.
