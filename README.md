# My AWS Automation Journey
Welcome to my repository, where I explore AWS automation through two distinct projects: a Lambda function deployment pipeline and a CloudFormation stack management system, both powered by GitHub Actions. This README takes you through my journey, detailing the purpose, setup, and workflows of these projects.

## Project 1: Lambda Function Deployment Pipeline
### Overview
The first project automates the deployment of an AWS Lambda function using GitHub Actions. The goal was to create a seamless CI/CD pipeline that triggers whenever I push changes to the Lambda function code from my local machine (via VS Code) to the remote GitHub repository.
### How It Works

Trigger: The workflow activates on any push to the main branch where changes are made to the Lambda function code (e.g., files in the lambda/ directory).
Workflow Steps:
Checks out the repository code.
Sets up the AWS CLI with credentials (securely stored in GitHub Secrets).
Packages the Lambda function code (e.g., zips the Python/Node.js files).
Deploys disbursed code to AWS Lambda using the AWS CLI or SDK.


Outcome: The Lambda function is updated in AWS without manual intervention, ensuring rapid iteration and deployment.

### My Journey
This project was born out of my desire to streamline Lambda development. Initially, I manually uploaded code to AWS, which was time-consuming and error-prone. By integrating GitHub Actions, I automated the process, learning how to configure AWS credentials securely and use the AWS CLI in a CI/CD context. The biggest challenge was debugging IAM permissions for the GitHub Actions runner, but it taught me the importance of least-privilege policies.

## Project 2: CloudFormation Stack Management with Pull Requests
### Overview
The second project focuses on infrastructure as code (IaC) using an AWS CloudFormation template (written in YAML) to manage an S3 bucket. The GitHub Actions workflow, defined in cf-validate-pr.yml, triggers on pull requests to validate and manage the CloudFormation stack.

### How It Works
Trigger: The workflow runs on pull request events (e.g., opened, synchronized, reopened, or closed) targeting the main branch, specifically when changes are made to files in the cloudformation/ directory.

Workflow Steps:
On Pull Request Creation/Update/Reopen

Validates the CloudFormation template (s3-bucket.yaml) using the AWS CLI.
Creates or updates a CloudFormation stack with an S3 bucket if validation passes.
Outputs the stack details (e.g., bucket name) for verification.

On Pull Request Close (Merge)

Triggers a cleanup job that deletes the CloudFormation stack from AWS.
Ensures no resources are left behind, keeping the AWS environment clean.


Outcome: Temporary infrastructure is created for testing during the pull request lifecycle and automatically removed after merging, reducing costs and manual cleanup.


### My Journey
This project was inspired by my need to test infrastructure changes safely before applying them permanently. I chose CloudFormation for its declarative approach and YAML for its readability. The pull request-based workflow, starting with cf-validate-pr.yml, was a game-changer, allowing me to validate infrastructure changes in a sandbox environment. The cleanup job was a critical addition after I noticed lingering stacks in AWS, teaching me about lifecycle management and cost optimization. Setting up the merge-triggered cleanup job required mastering GitHub Actions' event filters, which was a rewarding challenge.

## Repository Structure
.
├── .github/workflows/
│   ├── cf-validate-pr.yml  # CloudFormation validation and stack management workflow
│   └── lambbda.yml         # Lambda deployment workflow
├── cloudformation/
│   └── s3-bucket.yaml      # CloudFormation template for S3 bucket
├── lambda/
│   ├── lambdba_function.py # Example Lambda function
│   └── requirements.txt    # Dependencies
└── README.md               # This file

## Getting Started
To replicate this setup:

Clone the Repository:git clone <repository-url>

Configure AWS Credentials:
Store AWS access keys in GitHub Secrets (AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, AWS_REGION).

Lambda Project

Write your Lambda function in the lambda/ directory.
Push changes to trigger the lambbda.yml workflow.

CloudFormation Project

Modify s3-bucket.yaml in the cloudformation/ directory.
Create a pull request to trigger the cf-validate-pr.yml workflow.
Merge the pull request to clean up the stack.

## Lessons Learned

Automation Saves Time: Automating Lambda deployments reduced my deployment time from minutes to seconds.
IaC is Powerful: CloudFormation made infrastructure repeatable and testable, but required careful validation.
GitHub Actions Flexibility: Event-driven workflows (push, pull request) enabled complex automation with minimal setup.
Clean Up Matters: Automated stack deletion prevented unexpected AWS costs and clutter.

## Future Improvements

Add unit tests for the Lambda function in the CI/CD pipeline.
Extend the CloudFormation template to include more AWS resources (e.g., DynamoDB, IAM roles).
Implement stack tagging for better resource tracking.
Explore AWS SAM for combined Lambda and CloudFormation management.

## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Conclusion
This repository represents my journey into AWS automation, combining code and infrastructure management with GitHub Actions. It’s been a learning experience in CI/CD, IaC, and cloud cost management. I hope this inspires you to explore similar automation workflows!
Feel free to contribute or reach out with questions!

