# stelios-ifttt-service

This project contains source code and supporting files for a serverless application that you can deploy with the SAM CLI. It includes the following files and folders.

- hello_world - Code for the application's Lambda function.
- events - Invocation events that you can use to invoke the function.
- tests - Unit tests for the application code. 
- template.yaml - A template that defines the application's AWS resources.

The application uses several AWS resources, including Lambda functions and an API Gateway API. These resources are defined in the `template.yaml` file in this project. You can update the template to add AWS resources through the same deployment process that updates your application code.

If you prefer to use an integrated development environment (IDE) to build and test your application, you can use the AWS Toolkit.  
The AWS Toolkit is an open source plug-in for popular IDEs that uses the SAM CLI to build and deploy serverless applications on AWS. The AWS Toolkit also adds a simplified step-through debugging experience for Lambda function code. See the following links to get started.

* [PyCharm](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [IntelliJ](https://docs.aws.amazon.com/toolkit-for-jetbrains/latest/userguide/welcome.html)
* [VS Code](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/welcome.html)
* [Visual Studio](https://docs.aws.amazon.com/toolkit-for-visual-studio/latest/user-guide/welcome.html)

## Deploy the sample application

The Serverless Application Model Command Line Interface (SAM CLI) is an extension of the AWS CLI that adds functionality for building and testing Lambda applications. It uses Docker to run your functions in an Amazon Linux environment that matches Lambda. It can also emulate your application's build environment and API.

To use the SAM CLI, you need the following tools.

* SAM CLI - [Install the SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html)
* [Python 3 installed](https://www.python.org/downloads/)
* Docker - [Install Docker community edition](https://hub.docker.com/search/?type=edition&offering=community)

To build and deploy your application for the first time, run the following in your shell:

```bash
sam build --use-container
sam deploy --guided
```

The first command will build the source of your application. The second command will package and deploy your application to AWS, with a series of prompts:

* **Stack Name**: The name of the stack to deploy to CloudFormation. This should be unique to your account and region, and a good starting point would be something matching your project name.
* **AWS Region**: The AWS region you want to deploy your app to.
* **Confirm changes before deploy**: If set to yes, any change sets will be shown to you before execution for manual review. If set to no, the AWS SAM CLI will automatically deploy application changes.
* **Allow SAM CLI IAM role creation**: Many AWS SAM templates, including this example, create AWS IAM roles required for the AWS Lambda function(s) included to access AWS services. By default, these are scoped down to minimum required permissions. To deploy an AWS CloudFormation stack which creates or modified IAM roles, the `CAPABILITY_IAM` value for `capabilities` must be provided. If permission isn't provided through this prompt, to deploy this example you must explicitly pass `--capabilities CAPABILITY_IAM` to the `sam deploy` command.
* **Save arguments to samconfig.toml**: If set to yes, your choices will be saved to a configuration file inside the project, so that in the future you can just re-run `sam deploy` without parameters to deploy changes to your application.

You can find your API Gateway Endpoint URL in the output values displayed after deployment.

## Use the SAM CLI to build and test locally

Build your application with the `sam build --use-container` command.

```bash
stelios-ifttt-service$ sam build --use-container
```

The SAM CLI installs dependencies defined in `hello_world/requirements.txt`, creates a deployment package, and saves it in the `.aws-sam/build` folder.

Test a single function by invoking it directly with a test event. An event is a JSON document that represents the input that the function receives from the event source. Test events are included in the `events` folder in this project.

Run functions locally and invoke them with the `sam local invoke` command.

```bash
stelios-ifttt-service$ sam local invoke HelloWorldFunction --event events/event.json
```

The SAM CLI can also emulate your application's API. Use the `sam local start-api` to run the API locally on port 3000.

```bash
stelios-ifttt-service$ sam local start-api
stelios-ifttt-service$ curl http://localhost:3000/
```

The SAM CLI reads the application template to determine the API's routes and the functions that they invoke. The `Events` property on each function's definition includes the route and method for each path.

```yaml
      Events:
        HelloWorld:
          Type: Api
          Properties:
            Path: /hello
            Method: get
```

## Add a resource to your application
The application template uses AWS Serverless Application Model (AWS SAM) to define application resources. AWS SAM is an extension of AWS CloudFormation with a simpler syntax for configuring common serverless application resources such as functions, triggers, and APIs. For resources not included in [the SAM specification](https://github.com/awslabs/serverless-application-model/blob/master/versions/2016-10-31.md), you can use standard [AWS CloudFormation](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html) resource types.

## Fetch, tail, and filter Lambda function logs

To simplify troubleshooting, SAM CLI has a command called `sam logs`. `sam logs` lets you fetch logs generated by your deployed Lambda function from the command line. In addition to printing the logs on the terminal, this command has several nifty features to help you quickly find the bug.

`NOTE`: This command works for all AWS Lambda functions; not just the ones you deploy using SAM.

```bash
stelios-ifttt-service$ sam logs -n HelloWorldFunction --stack-name stelios-ifttt-service --tail
```

You can find more information and examples about filtering Lambda function logs in the [SAM CLI Documentation](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-logging.html).

## Unit tests

Tests are defined in the `tests` folder in this project. Use PIP to install the [pytest](https://docs.pytest.org/en/latest/) and run unit tests.

```bash
stelios-ifttt-service$ pip install pytest pytest-mock --user
stelios-ifttt-service$ python -m pytest tests/ -v
```

## Cleanup

To delete the sample application that you created, use the AWS CLI. Assuming you used your project name for the stack name, you can run the following:

```bash
aws cloudformation delete-stack --stack-name stelios-ifttt-service
```

## Resources

See the [AWS SAM developer guide](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) for an introduction to SAM specification, the SAM CLI, and serverless application concepts.

Next, you can use AWS Serverless Application Repository to deploy ready to use Apps that go beyond hello world samples and learn how authors developed their applications: [AWS Serverless Application Repository main page](https://aws.amazon.com/serverless/serverlessrepo/)

# dev

## dynamo db locally
see 
- https://github.com/ganshan/sam-dynamodb-local
- https://github.com/lvthillo/aws-lambda-sam-demo, 
- https://stackoverflow.com/questions/48926260/connecting-aws-sam-local-with-dynamodb-in-docker
- https://github.com/rynop/abp-sam-nestjs
- for stream support: https://stackoverflow.com/questions/34883142/stream-support-for-local-dynamodb

```
#run in background
make local-run
# access the shell at http://localhost:8000/shell/

# use aws to manipulate the database
aws --profile=fake dynamodb list-tables --endpoint-url http://localhost:8000

AWS_PROFILE=fake ./dynamodb/create-local-table.sh

aws --profile=fake dynamodb  --endpoint-url http://localhost:8000 scan --table=TriggersTable

#add response

aws --endpoint-url http://localhost:8000 --profile=fake dynamodb update-item --table-name TriggersTable \
  --key '{"PK":{"S":"92429d82a41e93048"}}' \
  --update-expression 'SET triggerEvents = :i' \
  --expression-attribute-values file://<(echo '{ ":i" : {"S":"[{\"seqNo\": 1,\"data\": {\"instrument_name\":\"someName\",\"price\":\"10000\",\"instrument\":\"epic\",\"meta\": {\"id\": \"14b9-1fd2-acaa-5df5\",\"timestamp\": 1383597267}}}]"}}')

```

## test realtime alert
```
sam local invoke RealTimeAlerterFunction --event events/dynamodbUpdate.json
```
# cleanup 
```
make clean
```

# deploy
```
make build && AWS_PROFILE=ighackathon sam deploy --parameter-overrides 'ApiKey=XXXXX'
#or (using default account ighackathon)
make deploy API_KEY_SECRET_NAME=XXXXX 
#or 
AWS_PROFILE=ighackathon make deploy API_KEY_SECRET_NAME=stelios_igus_example/apikey 

```
## cloud 
### dynamoDb
```
#scan table
aws --profile=ighackathon dynamodb scan --table-name SteliosTest-Stelios-EpicPriceAlert
```

insert update

```
aws --profile=ighackathon dynamodb update-item --table-name SteliosTest-Stelios-EpicPriceAlert   --key '{"PK":{"S":"a1274ad5bc21ed2e02636e6bb1f8bc584ccb19f3"}}'   --update-expression 'SET triggerEvents = :i'   --expression-attribute-values file://<(echo '{ ":i" : {"S":"[{\"seqNo\": 1,\"data\": {\"instrument_name\":\"someName\",\"price\":\"10000\",\"instrument\":\"epic\",\"meta\": {\"id\": \"14b9-1fd2-acaa-5df5\",\"timestamp\": 1383597267}}}]"}}')
```
### logs
logs:

```
AWS_PROFILE=ighackathon sam logs  --name RealTimeAlerterFunction --stack-name SteliosTest --tail

AWS_PROFILE=ighackathon sam logs  --name TriggerFunction --stack-name SteliosTest --tail
```
