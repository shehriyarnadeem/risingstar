# Getting Started with Create React App


1-AWS Codecommit repository as code source

2- AWS Codebuild to build code

3- AWS CodeDeploy to deploy code

4- AWS S3 bucket to host website

5- AWS CodePipeline for CI\CD


# Step 1
## Pushing Code to CodeCommit

Now we will create a repository on CodeCommit. We will navigate to Codecommit from management console and create a new repository. After creation clone your repo to your local system or either upload the files and commit them. You can Copy paste these files from your react app folder, commit and push it to your code commit repository. You should have following files.

Here you are looking at a strange file buildspec.yml.

We will have to create buildspec.yml file on our own. The creation of this file mandatory. It contains all the build specifications required for building the project through codebuild.

In this file we have defined the build stages and commands to run at each stage. You can get this file from git repository link shared. Take care of the indentation in this file otherwise it won’t work.


# Step 2:

## Creation of S3 Bucket for Hosting

Now we will go to our AWS management console and create S3 bucket for hosting static website. After creation of bucket open it and open it’s properties.

In properties tab of S3 Bucket go to “Static website hosting” and select the check box “Use this bucket to host website”. Enter “index.html” in Index document text box and “error.html” in Error document. As shown below.

After that go to Permissions tab and change Block all public access to off.


After that go to “Bucket Policy” tab and insert following policy and save it. Enter the name of your bucket at resource attribute by replacing “your_bucket_name” with bucket name.

  {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::risingstarbucket/*"
        }
    ]
}

Finally go to “CORS configuration” tab and insert this information and save it.

<?xml version=”1.0" encoding=”UTF-8"?>
<CORSConfiguration xmlns=”http://s3.amazonaws.com/doc/2006-03-01/">
<CORSRule>
<AllowedOrigin>*</AllowedOrigin>
<AllowedMethod>GET</AllowedMethod>
</CORSRule>
</CORSConfiguration>


# Step 3:

## Creation of Codepipeline

After setting up S3 bucket. We will select codepipeline from AWS management console and open the service. We will select create pipeline and Name our pipeline. We will select these settings and press next.

Source for pipline: Next it will ask for source provider, as our source is AWS Codecommit we will select that and click on connect to codecommit. We will select our required repository and it’s branch where we have committed our react app code . Press next.

Create build Project: After that it asks for build details we will select AWS codebuild and select “create a project” it will open a window to create a new project.

Here We will write name of the project.

We will create a codebuild project with Amazon Linux 2 as Operating System, Runtime as Standard and image as 3.0. As below

Select New service role. After that Select Use a build file as we have buildspec.yml file in our git repository. As below.

No more changes required click on continue to pipeline. Then click next.

Deploy stage: Then we add a deploy stage where we select our S3 bucket that we created earlier to host website. Press next

Click on create pipeline and that will create the codepipeline.

Stages of Pipeline
The pipeline will have 3 stages. Failure of any one stage will rollback all.

Source Stage

First one will be Source stage where our code will be automatically pulled from codecommit repository.

Build Stage

Second stage will be the Build stage, after pulling code from codecommit repo codebuild will build the code.

Deploy Stage

In the third step after successfully building the code, it will be deployed to S3. Through the Deploy stage.




# Step 4:
## Running the website:

Now we go back to our S3 bucket. In the bucket we go to the properties tab. Inside properties tab we navigate to Static website hosting tab and click on the Endpoint link available for our website.
