### Step-by-Step Deployment Guide for Lambda to ECR

1. **Build the Docker Image Using Docker BuildX:**
   - **Goal:** Create a cross-platform Docker image compatible with ARM64 for Mac M1.
   - **Procedure:**
     ```sh
     docker buildx build --platform linux/arm64 -t whisper:latest .
     ```

2. **Test the Docker Container Locally:**
   - **Goal:** Ensure the Docker container works correctly and identify any credential issues.
   - **Procedure:**
     ```sh
     docker run -p 9000:8080 whisper:latest
     ```
   - If you encounter an 'unable to locate credential' error, proceed to the next step.

3. **Mount AWS Credentials to the Docker Container:**
   - **Goal:** Provide the Docker container with access to AWS credentials.
   - **Procedure:**
     ```sh
     docker run -v ~/.aws:/root/.aws -p 9000:8080 whisper:latest
     ```

4. **Tag the Docker Image for ECR:**
   - **Goal:** Prepare the Docker image for upload to Amazon Elastic Container Registry (ECR).
   - **Procedure:**
     ```sh
     docker tag whisper:latest <aws_account_id>.dkr.ecr.<region>.amazonaws.com/whisper:latest
     ```

5. **Login to Amazon ECR:**
   - **Goal:** Authenticate Docker to your Amazon ECR registry.
   - **Procedure:**
     ```sh
     aws ecr get-login-password --region <region> | docker login --username AWS --password-stdin <aws_account_id>.dkr.ecr.<region>.amazonaws.com
     ```

6. **Push the Docker Image to ECR:**
   - **Goal:** Upload your Docker image to Amazon ECR.
   - **Procedure:**
     ```sh
     docker push <aws_account_id>.dkr.ecr.<region>.amazonaws.com/whisper:latest
     ```

7. **Create a Lambda Function from the ECR Image:**
   - **Goal:** Deploy the Docker image as a Lambda function.
   - **Procedure:**
     - Go to the AWS Lambda console.
     - Click on "Create function".
     - Select "Container image" and configure the function details.
     - In the "Image URI" field, enter the ECR image URI (e.g., `<aws_account_id>.dkr.ecr.<region>.amazonaws.com/whisper:latest`).
     - Configure the necessary execution role and other settings.
     - Click "Create function".

8. **Test the Lambda Function:**
   - **Goal:** Ensure the Lambda function works as expected.
   - **Procedure:**
     - Create a test event in the Lambda console.
     - Run the test and verify the output logs.
     - Monitor for any issues and troubleshoot if necessary.

By following these steps, you will successfully build, test, and deploy a Docker image to AWS Lambda using ECR, ensuring smooth and efficient deployment of your application.