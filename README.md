# FaaS Face Recognition

![FaaS Face Recognition](https://github.com/JaynilVaidya/FaaS-Face-Recognition/raw/main/FaaS_Face_Recognition.png)

## Overview

Welcome to the FaaS Face Recognition project! This repository hosts an elastic application designed to process videos through a multi-stage pipeline using AWS Lambda and other supporting services from AWS. Leveraging the power of Function as a Service (FaaS) and Platform as a Service (PaaS) cloud computing, this application offers a scalable and cost-effective solution for video analysis, specifically focusing on facial recognition tasks.

## Key Components

### 1. video-splitting Function

- **Description:** Splits uploaded videos into frames and chunks them into the group-of-pictures (GoP) using FFmpeg.
- **Triggers:** Triggered whenever a new video is uploaded to a designated input bucket.
- **Output:** Stores one frame per video in an intermediate bucket and asynchronously invokes the face-recognition function.

### 2. face-recognition Function

- **Description:** Detects and extracts faces from frames using OpenCV APIs and performs facial recognition using a pre-trained CNN model (ResNet-34).
- **Triggers:** Triggered upon completion of the video-splitting function.
- **Output:** Stores the name of the recognized person in text files in an output bucket.

## Deployment

To deploy FaaS Face Recognition, follow these steps:

1. **AWS Setup**: Ensure you have an AWS account and have the AWS Command Line Interface (CLI) installed on your machine.

2. **Create S3 Buckets**: Create three S3 buckets for input, intermediate, and output storage. You can name the buckets according to your preference, ensuring they adhere to S3 naming conventions.

3. **Dockerfile Configuration**:
   - Use the provided Dockerfile in the repository to build a container image for the face-recognition Lambda function.
   - Customize the Dockerfile as needed to include any additional dependencies required for your face-recognition function.

4. **Deploy Lambda Functions**:
   - Create Lambda functions for video-splitting and face-recognition using the AWS Management Console or AWS CLI.
   - Upload the container images built from the Dockerfiles for each function to AWS Lambda.

5. **Configure Permissions**:
   - Set up appropriate triggers for the Lambda functions to execute based on bucket events.
   - Ensure the functions have the necessary permissions to access the S3 buckets created in step 2.

6. **Testing**:
   - Test the deployment by uploading a video file to the input bucket.
   - Monitor the execution of Lambda functions in the AWS Management Console to ensure they process the video successfully.

7. **Customization**:
   - Customize the Lambda functions and configurations according to your specific requirements.
   - Adjust parameters such as frame extraction frequency, facial recognition algorithms, and output formats as needed.

## Contact

For any inquiries or feedback, please contact the project maintainer:

- **Name:** Jaynil Vaidya
- **Email:** [vaidyajaynil@gmail.com](mailto:vaidyajaynil@gmail.com)
