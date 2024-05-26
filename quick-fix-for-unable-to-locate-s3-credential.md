### Quick Fix for 'Unable to Locate Credential' Error

1. **Build the Docker Image Using Docker BuildX:**
   ```sh
   docker buildx build --platform linux/arm64 -t whisper:latest .
   ```

2. **Run the Container Without AWS Credentials to Reproduce the Error:**
   ```sh
   docker run -p 9000:8080 whisper:latest
   ```
   This will show the 'unable to locate credential' error.

3. **Run the Container with Mounted AWS Credentials:**
   ```sh
   docker run -v ~/.aws:/root/.aws -p 9000:8080 whisper:latest
   ```
   This command mounts your AWS credentials into the Docker container, resolving the error.