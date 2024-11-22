Overview:

This project demonstrates how I automated the deployment of a simple memory card game using AWS CodePipeline and S3. The project highlights how to seamlessly integrate GitHub as the source repository and S3 as the deployment destination for static website hosting.

Through this hands-on project, I implemented a fully functional CI/CD pipeline that enables code updates in GitHub to trigger an automatic deployment to an S3 bucket configured for static website hosting.


*** Key Objectives:

Host a static website (the memory card game) on S3.
Build and configure an AWS CodePipeline to automate deployment from GitHub to S3.
Understand pipeline stages: Source, Build, and Deploy.
Apply best practices for resource cleanup and ensure cost-effectiveness.

*** Workflow:
1. Fork the GitHub Repository
I started by forking the game’s code repository to my own GitHub account. This repository contains the source code for the memory card game, including:

HTML: Game layout.
CSS: Styling the game elements.
JavaScript: Logic for gameplay.
Images: Meme cards for the game.

*** Thanks to @tinytutorials for the original repository.

Configure an S3 Bucket for Static Website Hosting:

Next, I created an S3 bucket to host the static files for the game. I configured the bucket for public access and enabled Static Website Hosting.

Steps:

Navigate to the S3 Console in AWS.
Click Create Bucket and configure:
Bucket Name: memory-game-hosting (replace with a unique name).
Region: Choose your desired AWS region.
Under Permissions, uncheck "Block all public access" to make the bucket publicly accessible.
Enable Static Website Hosting:
Go to Properties → Static Website Hosting.
Specify the index document as index.html.

*** Create a New CodePipeline:

The pipeline automates the process of fetching code changes from GitHub and deploying them to the S3 bucket.

Steps:
Navigate to the CodePipeline Console.
Click Create Pipeline and configure the following:
Pipeline Settings:
Pipeline Name: MemoryGamePipeline.
Service Role: Use an existing role or create a new one with required permissions.
Source Stage:
Provider: GitHub.
Repository: Connect to my forked GitHub repository.
Branch: Choose the branch (e.g., main) to monitor for changes.

Build Stage:
Since this is a static website, I skipped the build stage.

Deploy Stage:
Provider: Amazon S3.
Bucket Name: memory-game-hosting.
Object Key: Leave this blank to deploy all files.
Save and create the pipeline.

*** Test the Static Website:

Once the pipeline was created, it automatically pulled the code from GitHub and deployed it to the S3 bucket. I tested the website by accessing the S3 Static Website Hosting Endpoint.

Testing Steps:
Navigate to the S3 bucket's Properties → Static Website Hosting.
Copy the Endpoint URL (e.g., http://memory-game-hosting.s3-website-us-east-1.amazonaws.com).
Open the URL in a browser to play the game!

*** Triggering the Pipeline with Code Changes:

To test the pipeline’s automation, I made a small change to the index.html file in my GitHub repository and committed the update.

Observations:
CodePipeline detected the commit, pulled the updated code, and redeployed it to the S3 bucket.
Refreshing the website showed the updated changes live.

** Challenges I faced:

While connecting GitHub to CodePipeline, I faced issues with authentication and setting up the webhook. The pipeline could not detect changes from the GitHub repository. So I had to ensured that my GitHub repository was public (or I added the appropriate OAuth token for private repos). I provided CodePipeline with permissions to access the repository. I verified that the webhook was correctly set up by checking the repository’s Webhooks settings in GitHub.
Also at one point, the deployment stage of the pipeline was taking longer than expected, causing delays in the testing process. The issue was due to large files being uploaded to the S3 bucket. To optimize the deployment process I ensured that unnecessary files (like large images) were removed from the repository, and ompressed the remaining assets, reducing the overall upload time.
Furthermore, on the pipeline, there were times when the pipeline failed at the deploy stage, but the error messages were not immediately clear. I utilized the logs available in the CodePipeline Console and CloudWatch to troubleshoot the issues. This taught me how to interpret pipeline logs and take corrective actions effectively.

These challenges not only helped me understand AWS services better but also taught me how to troubleshoot and optimize deployments in a real-world environment.

*** Lessons Learned
This project helped me:

- Understand the end-to-end deployment process using AWS CodePipeline.
- Configure S3 for static website hosting with proper permissions.
- Automate deployments effectively using CI/CD principles.
- Debug issues in pipeline stages and understand pipeline logs.
  
*** Future Enhancements:
- Add a Build Stage: Integrate a build process to minify and optimize files before deployment.
- Add Monitoring: Use AWS CloudWatch to monitor the pipeline and website health.
- Enhance the Game: Improve the UI/UX of the game or add more gameplay features



The game consists of HTML, CSS and JavaScript.

Ideas for additional features:
- A scoring mechanism
- A timer
- Add additional cards
- Multi-player capabilities so you can compare scores 

## The Deployment Environment
The code will be deployed and hosted in S3.

## The Deployment Pipeline
The pipeline is created using AWS Code Pipeline.  The pipeline pulls the code from GitHub, and deploys it to S3 whenever a change is detected in the code.

