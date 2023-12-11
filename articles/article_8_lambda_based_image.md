Creating an AWS Lambda function based on a container image involves several steps, including building the image,
uploading it to Amazon Elastic Container Registry (Amazon ECR), and configuring the Lambda function to use the image.
Here's a step-by-step guide:

AWS launched the python3.11 support for Lambda. Lambda Python3.11 PostgreSQL images using Amazon Linux 2 are available
here (https://gallery.ecr.aws/lambda/python). Support for earlier Python versions may end soon.

Prerequisites:

An AWS account with IAM user access
Docker installed and configured
Familiarity with container image concepts
Step 1: Build the Docker Image

Create a Dockerfile in the directory containing your application code.
Specify the base image for your application. You can use an AWS-provided base image or a custom image from another
registry.
Install any necessary dependencies for your application.
Copy your application code into the image.
Define the entrypoint for your application.
Step 2: Create an Amazon ECR Repository

Navigate to the Amazon ECR console in your AWS Management Console.
Click on "Create Repository".
Provide a repository name and select the appropriate region.
Click on "Create Repository" to create the repository.
Step 3: Push the Docker Image to Amazon ECR

Tag the Docker image with the Amazon ECR repository URI. Replace <repository-uri> with the actual URI from your
repository:
Bash
docker tag <image-name>:<image-tag> <repository-uri>
Use code with caution. Learn more
Push the tagged image to Amazon ECR:
Bash
docker push <repository-uri>
Use code with caution. Learn more
Step 4: Create the AWS Lambda Function

Navigate to the AWS Lambda console in your AWS Management Console.
Click on "Create function".
Select "Container Image" as the function type.
Provide a function name and description.
Under "Container Image", specify the Amazon ECR repository URI you created earlier.
Configure any additional function settings, such as memory, timeout, and execution role.
Click on "Create function" to create the Lambda function.
Step 5: Test the AWS Lambda Function

Click on the newly created Lambda function in the console.

Click on the "Test" tab.

Provide any input data required by your function.

Click on "Test" to execute the function.

Review the test results to ensure the function is working as expected.
