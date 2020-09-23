https://www.youtube.com/watch?v=71cd5XerKss&t=2s

$ sudo npm install -g serverless

```
$ serverless create
Serverless: Generating boilerplate...
 
  Serverless Error ---------------------------------------
 
  You must either pass a template name (--template), a URL (--template-url) or a local path (--template-path).
 
  Get Support --------------------------------------------
     Docs:          docs.serverless.com
     Bugs:          github.com/serverless/serverless/issues
     Issues:        forum.serverless.com
 
  Your Environment Information ---------------------------
     Operating System:          linux
     Node Version:              12.18.3
     Framework Version:         2.2.0
     Plugin Version:            4.0.4
     SDK Version:               2.3.2
     Components Version:        3.1.4

https://www.serverless.com/framework/docs/providers/aws/cli-reference/create/

Available Templates
To see a list of available templates run serverless create --help

Most commonly used templates:

aws-clojurescript-gradle
aws-clojure-gradle
aws-nodejs
aws-nodejs-typescript
aws-alexa-typescript
aws-nodejs-ecma-script
aws-python
aws-python3
aws-ruby
aws-provided
aws-kotlin-jvm-maven
aws-kotlin-jvm-gradle
aws-kotlin-nodejs-gradle
aws-groovy-gradle
aws-java-maven
aws-java-gradle
aws-scala-sbt
aws-csharp
aws-fsharp
aws-go
aws-go-dep
aws-go-mod
plugin

https://www.serverless.com/framework/docs/providers/aws/examples/hello-world/
https://www.serverless.com/framework/docs/providers/aws/examples/hello-world/python/
https://www.serverless.com/framework/docs/providers/aws/examples/hello-world/node/

$ serverless create --template aws-nodejs
Generates handler.js and serverless.yml

$ serverless create --template aws-java-maven
Generates pom.xml and serverless.yml

$ serverless create --template aws-python3
Generates handler.py and serverless.yml

Whenever a trigger event happens, a hello function in handler.js will run.

serverless.yml configures this function.

serverless.yml:

Changed service: serverless-example to
service: lambda-test

Uncomment events:
#    events:
#      - http:
#          path: users/create
#          method: get

Runs on the s3 bucket changes (whenever a file is uploade to the s3 bucket - e.g. whenever a new image is uploaded, lets resize the image)
#      - s3: ${env:BUCKET}

Runs on schedule:
#      - schedule: rate(10 minutes)

Access keys
Log into https://aws.amazon.com/console/ as IAM user
Click Administrator on the toolbar on the right
From the drop-down, select My Security Credentials
Scroll down to Access keys for CLI, SDK, & API access
Click Create Access Key
Copy Access Key ID: xxx
Secret access key: ***

$ serverless config credentials --provider aws --key xxx --secret ***
Serverless: Setting up AWS...
[alex@alex-pc serverless-example]$ 

Credentials are stored now in ~/.aws/credentials

[alex@alex-pc serverless-example]$ serverless deploy
Serverless: Packaging service...
Serverless: Excluding development dependencies...
Serverless: Creating Stack...
Serverless: Checking Stack create progress...
........
Serverless: Stack create finished...
Serverless: Uploading CloudFormation file to S3...
Serverless: Uploading artifacts...
Serverless: Uploading service lambda-test.zip file to S3 (10.46 KB)...
Serverless: Validating template...
Serverless: Updating Stack...
Serverless: Checking Stack update progress...
.................................
Serverless: Stack update finished...
Service Information
service: lambda-test
stage: dev
region: us-east-1
stack: lambda-test-dev
resources: 12
api keys:
  None
endpoints:
  GET - https://jj5h2lfns7.execute-api.us-east-1.amazonaws.com/dev/users/create
functions:
  hello: lambda-test-dev-hello
layers:
  None

**************************************************************************************************************************************
Serverless: Announcing Metrics, CI/CD, Secrets and more built into Serverless Framework. Run "serverless login" to activate for free..
**************************************************************************************************************************************

[alex@alex-pc serverless-example]$ 

handler.js:
    body: JSON.stringify(
      {
        message: 'Go Serverless v1.0! Your function executed successfully!',
        input: event,
      },
      null,
      2
    ),

https://jj5h2lfns7.execute-api.us-east-1.amazonaws.com/dev/users/create

{
  "message": "Go Serverless v1.0! Your function executed successfully!",
  "input": {
    "resource": "/users/create",
    "path": "/users/create",
    "httpMethod": "GET",
    "headers": {
      "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9",
      "Accept-Encoding": "gzip, deflate, br",
      "Accept-Language": "en-US,en;q=0.9",
      "CloudFront-Forwarded-Proto": "https",
      "CloudFront-Is-Desktop-Viewer": "true",
      "CloudFront-Is-Mobile-Viewer": "false",
      "CloudFront-Is-SmartTV-Viewer": "false",
      "CloudFront-Is-Tablet-Viewer": "false",
      "CloudFront-Viewer-Country": "US",
      "Host": "jj5h2lfns7.execute-api.us-east-1.amazonaws.com",
      "sec-fetch-dest": "document",
      "sec-fetch-mode": "navigate",
      "sec-fetch-site": "none",
      "upgrade-insecure-requests": "1",
      "User-Agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.121 Safari/537.36",
      "Via": "2.0 2772ea7c91d6d2b9d83ea6d082faecc9.cloudfront.net (CloudFront)",
      "X-Amz-Cf-Id": "e55J4NbLbGFU8_rOV5V4nRPwrCn3yO5rc05Gowo-pwnlVxJZmKS8Ag==",
      "X-Amzn-Trace-Id": "Root=1-5f6bbfbb-01ca0ad06c6e3f00c5a92880",
      "X-Forwarded-For": "173.48.212.235, 130.176.29.86",
      "X-Forwarded-Port": "443",
      "X-Forwarded-Proto": "https"
    },

$ serverless deploy --stage production

serverless.yml:
before:
functions:
  hello:
    handler: handler.hello
    events:
     - http:
         path: users/create
         method: get

after:
functions:
  hello:
    handler: handler.hello
    events:
     - http:
         path: /
         method: get
  imageResize:
    handler: handler.hello
    events:
     - http:
         path: /imageResize
         method: get

handler.js:
before:
module.exports.hello = async event => {
  return {
    statusCode: 200,
    body: JSON.stringify(
      {
        message: 'v1.0',
        input: event,
      },
      null,
      2
    ),
  };

  // Use this code if you don't use the http event with the LAMBDA-PROXY integration
  // return { message: 'Go Serverless v1.0! Your function executed successfully!', event };
};

after:
module.exports.hello = async event => {
  return {
    statusCode: 200,
    body: JSON.stringify(
      {
        message: 'v1.0',
        input: event,
      },
      null,
      2
    ),
  };

  // Use this code if you don't use the http event with the LAMBDA-PROXY integration
  // return { message: 'Go Serverless v1.0! Your function executed successfully!', event };
};

module.exports.imageResize = async event => {
  return {
    statusCode: 200,
    body: JSON.stringify(
      {
        message: 'Resize your image!',
        input: event,
      },
      null,
      2
    ),
  };

  // Use this code if you don't use the http event with the LAMBDA-PROXY integration
  // return { message: 'Go Serverless v1.0! Your function executed successfully!', event };
};

$ serverless deploy --stage production
Serverless: Packaging service...
Serverless: Excluding development dependencies...
Serverless: Uploading CloudFormation file to S3...
Serverless: Uploading artifacts...
Serverless: Uploading service lambda-test.zip file to S3 (11.8 KB)...
Serverless: Validating template...
Serverless: Updating Stack...
Serverless: Checking Stack update progress...
......................
Serverless: Stack update finished...
Service Information
service: lambda-test
stage: production
region: us-east-1
stack: lambda-test-production
resources: 16
api keys:
  None
endpoints:
  GET - https://5bm022ei84.execute-api.us-east-1.amazonaws.com/production/
  GET - https://5bm022ei84.execute-api.us-east-1.amazonaws.com/production/imageResize
functions:
  hello: lambda-test-production-hello
  imageResize: lambda-test-production-imageResize
layers:
  None

********************************************************************************************************************************************
Serverless: Announcing an enhanced experience for Serverless Full-Stack Applications: https://github.com/serverless-components/fullstack-app
********************************************************************************************************************************************

https://5bm022ei84.execute-api.us-east-1.amazonaws.com/production/
{
  "message": "v1.0",
  "input": {
    "resource": "/",
    "path": "/",
    "httpMethod": "GET",
    "headers": {
      "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9",
      "Accept-Encoding": "gzip, deflate, br",
      "Accept-Language": "en-US,en;q=0.9",
      "CloudFront-Forwarded-Proto": "https",
      "CloudFront-Is-Desktop-Viewer": "true",
      "CloudFront-Is-Mobile-Viewer": "false",
      "CloudFront-Is-SmartTV-Viewer": "false",
      "CloudFront-Is-Tablet-Viewer": "false",
      "CloudFront-Viewer-Country": "US",
      "Host": "5bm022ei84.execute-api.us-east-1.amazonaws.com",
      "sec-fetch-dest": "document",
      "sec-fetch-mode": "navigate",
      "sec-fetch-site": "none",
      "upgrade-insecure-requests": "1",
      "User-Agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.121 Safari/537.36",
      "Via": "2.0 a0b94a243c49df97658a8a3ea0fe2d20.cloudfront.net (CloudFront)",
      "X-Amz-Cf-Id": "HTMAv8OK6-_F7q3fscysluI8IG2i7jmwAeV7eEObkKOl9b985U6P3w==",
      "X-Amzn-Trace-Id": "Root=1-5f6bc6c8-65ef63543919a827f90773e8",
      "X-Forwarded-For": "173.48.212.235, 130.176.29.97",
      "X-Forwarded-Port": "443",
      "X-Forwarded-Proto": "https"
    },

https://5bm022ei84.execute-api.us-east-1.amazonaws.com/production/imageResize
{
  "message": "Resize your image!",
  "input": {
    "resource": "/imageResize",
    "path": "/imageResize",
    "httpMethod": "GET",
    "headers": {
      "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9",
      "Accept-Encoding": "gzip, deflate, br",
      "Accept-Language": "en-US,en;q=0.9",
      "CloudFront-Forwarded-Proto": "https",
      "CloudFront-Is-Desktop-Viewer": "true",
      "CloudFront-Is-Mobile-Viewer": "false",
      "CloudFront-Is-SmartTV-Viewer": "false",
      "CloudFront-Is-Tablet-Viewer": "false",
      "CloudFront-Viewer-Country": "US",
      "Host": "5bm022ei84.execute-api.us-east-1.amazonaws.com",
      "sec-fetch-dest": "document",
      "sec-fetch-mode": "navigate",
      "sec-fetch-site": "none",
      "upgrade-insecure-requests": "1",
      "User-Agent": "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.121 Safari/537.36",
      "Via": "2.0 a0b94a243c49df97658a8a3ea0fe2d20.cloudfront.net (CloudFront)",
      "X-Amz-Cf-Id": "U0vELGrFKI14qM-zGaCNGjHqrH2mjRbPHwlvcXSbTt8xUCRls0-1Jw==",
      "X-Amzn-Trace-Id": "Root=1-5f6bc709-16545579abdb7dcf15e60a68",
      "X-Forwarded-For": "173.48.212.235, 130.176.29.146",
      "X-Forwarded-Port": "443",
      "X-Forwarded-Proto": "https"
    },    

https://aws.amazon.com/
https://console.aws.amazon.com/ 
Click Lambda under All services
Functions:
lambda-test-production-hello
lambda-test-production-imageResize
lambda-test-dev-hello  
```



