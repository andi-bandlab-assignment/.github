# System Design

![architecture](https://github.com/andi-bandlab-assignment/.github/assets/10074400/79e6c628-ece5-4a5f-8178-ea1184e78a98)

## Overview
This document outlines the serverless architecture for a given assignment about social media application, 
focusing on image and post management.

## Components
- Client: The front-end interface for user interactions.
- API Gateway: Manages incoming requests and routes them to the correct services.
- Image-upload: Uploads images and triggers processing events.
- User-post-images: Stores processed images.
- Image-resize: Resizes images to meet application requirements.
- Post-creation: Creates posts with images and metadata.
- User-post: A database for storing post details.
- Post-retrieval: Retrieves posts for viewing.
- Comment-handler: Manages creation and deletion of comments on posts.

## Workflow
1. Uploading an Image:
- A POST request to `/upload` sends the image to the image-upload service and returns a presigned URL.
- Once the image is uploaded via PUT request to the presigned URL, it triggers image-resize for image processing.

2. Post Creation:
- A POST request to `/posts` triggers the post-creation service.
- The service checks if the resized image is available in user-post-images.
- Once verified, it stores the post details in the user-post database.
- If the image is not available, it would fallback to a default image in the same bucket.

3. Post Retrieval:
- A GET request to `/posts` retrieves the list of posts from post-retrieval.

4. Comment Management:
- A POST request to `/comments` adds a comment to a post through comment-handler.
- A DELETE request to `/comments/{comment_id}/creators/{creator_id}` removes a comment belongs to the creator by the creator.

## API Docs
Swagger API: https://andi-bandlab-assignment.github.io/swagger-api-docs/#/

If the Swagger API does not work, you may try using Postman collection: https://web.postman.co/workspace/My-Workspace~099df822-3d93-4910-888d-bc5e8f6efd36/request/11655290-6378f31a-c614-4b5e-8a15-cdf296259d6b

## What's Next
This MVP is not production-ready. In order to ship to production, we have to complete the following:
1. Unit Test in all services. Currently only `image-upload` has unit tests.
2. CI/CD pipeline which include test and publish-zip stages.
3. Terraform resources for all required AWS resources and permissions, e.g. Lambda, IAM role, DynamoDB, etc.
4. Deployment via Jenkins job by specifying the Terraform resources, or Terraform CLI which includes Terraform init, plan, and apply.