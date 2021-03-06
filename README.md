# yolo

Manage infrastructure and services on AWS for multiple accounts/stages.

(Or, "yolo everything into prod.")

## Terminology

 * __Project:__ a collection of services, the code repository represents a project.
 * __Service:__ typically a microservice, could be an API Gateway/Lambda powered API, or a CloudFront-based web UI. A project may contain multiple services.
 * __Account:__ an AWS account, in some contexts it also means a baseline [infrastructure].
 * __Stage:__ an instance of the project deployed to an account. There may be multiple stages within an account. Examples of stages are "dev", "test", "QA", "production", etc.

## Docker build container for Lambda Functions

The `Dockerfile` contained in this repo is published to
https://hub.docker.com/r/larsbutler/yolo/. It is built directly from GitHub, so
if this file is updated the patch needs to merged downstream into the master
branch of https://github.com/larsbutler/yolo.

## Workflow

 0. Authenticate with "Fanatical Support for AWS" (using Rackspace Cloud credentials), so that AWS account credentials can be fetched when needed, without having to re-authenticate:

```
yolo login
```

 1. Create a Yolo file, you can find [an example](https://github.com/rackerlabs/yolo/blob/master/example.yolo.yaml) in the code repository.
 2. Define AWS account-level resources using a CloudFormation template.
 3. Deploy the baseline infrastructure:

```
yolo deploy-baseline-infra --account testaccount
```

 4. Define stage-level resources using a different CloudFormation template. There can be multiple stages within a single account (they will be deployed as separate CloudFormation stacks), and a stage represents an instance of your project (collection of services).
 5. Deploy the stage-level infrastructure:

```
yolo deploy-infra --stage dev
```

 6. Depending on the type of application you're developing, you can finally build and/or deploy it:

```
yolo build-lambda --stage dev
yolo deploy-lambda --stage dev
```

## All supported commands

```
Usage: yolo [OPTIONS] COMMAND [ARGS]...

  Manage infrastructure and services on AWS for multiple accounts/stages.

  (Or, "yolo everything into prod".)

Options:
  -h, --help  Show this message and exit.

Commands:
  build-lambda           Build Lambda function packages.
  clear-config           Clear cached configuration for `yolo`.
  deploy-baseline-infra  DEPRECATED: Use `yolo deploy-infra` instead.
  deploy-infra           Deploy infrastructure from templates.
  deploy-lambda          Deploy Lambda functions for services.
  deploy-s3              Deploy a built S3 application.
  list-accounts          List AWS accounts.
  list-builds            List the pushed builds for a service/stage.
  list-lambda-builds     DEPRECATED: Use `yolo list-builds` instead.
  list-s3-builds         DEPRECATED: Use `yolo list-builds` instead.
  login                  Login with and cache Rackspace credentials.
  push                   Push a local build, ready it for deployment.
  push-lambda            DEPRECATED: Use `yolo push` instead.
  put-parameters         Securely store service/stage parameters.
  run                    Run a script with AWS account credentials.
  shell                  Launch a Python shell with AWS credentials.
  show-config            Show currently cached configuration.
  show-outputs           Show infrastructure stack outputs.
  show-parameters        Show centralized config for a service/stage.
  show-service           Show service configuration for a given stage.
  status                 Show infrastructure deployments status.
  upload-s3              DEPRECATED: Use `yolo push` instead.
```
