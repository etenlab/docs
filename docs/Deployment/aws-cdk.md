# AWS CDK (INFRASTRUCTURE)

Project infrastructure written with Typescript CDK.

## Project structure

The project consists of several CloudFormation stacks:

1. `1 * CommonStack` - deploys resources shared across all applications (VPC, ALB, ECS Cluster, Route53 Hosted Zone)
2. `1 * DatabaseStack` - deploys Aurora PostgreSQL cluster shared across all applications
3. `N * ApiServiceStack` - deploys project APIs hosted on ECS
4. `N * FrontendStack` - deploys project frontends hosted on S3 + CloudFront

## First-time deployment

1. Configured your AWS CLI with correct credentials. See [AWS CLI Configuration basics](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html) for reference.
   - Install AWS CDK Toolkit (cdk command). See (https://docs.aws.amazon.com/cdk/v2/guide/cli.html) for refetence.
2. Bootstrap CDK project in your AWS account if you have not done so already. See [CDK Bootstrapping docs](https://docs.aws.amazon.com/cdk/v2/guide/bootstrapping.html) for reference.
3. Install project dependencies: `npm ci`.
4. Check environment configuration in `./config/dev.yaml` for development environment. Use `./config/prod.yaml` for production.
5. Deploy Common stack: `cdk deploy -c env=dev devCommonStack`
6. Deploy Database stack `cdk deploy -c env=dev devDatabaseStack`
7. Deploy other stack/s of your choice:
   - single stack: `cdk deploy -c env=dev devCommonStack`.
   - several stacks: `cdk deploy -c env=dev devDatabaseApiStack, devAdminApiStack`.

## Hosted zone configuration

This project assumes that root hosted zone i.e. `crowd.bible` already exists in the AWS account.

If you also need to create a subdomain for environment, i.e. `dev.crowd.bible`, please make sure `./config/dev.yaml` includes the following:

```yaml
# Env domain setup
createEnvHostedZone: true
rootDomainName: 'crowd.bible'
envDomainName: 'dev.crowd.bible'
```

If you don't plan to use subdomains and want to add records to the root hosted zone directly, i.e. `api.crowd.bible`, please make sure ARR of ACM certificate for the root domain is provided:

```yaml
# Env domain setup
createEnvHostedZone: false
rootDomainName: 'crowd.bible'
rootDomainCertArn: 'arn:aws:acm:us-east-2:579742570368:certificate/000000000'
```

## Useful commands

- `npm run build` compile typescript to js
- `npm run watch` watch for changes and compile
- `npm run test` perform the jest unit tests
- `cdk deploy` deploy this stack to your default AWS account/region
- `cdk diff` compare deployed stack with current state
- `cdk synth` emits the synthesized CloudFormation template
- `cdk context --clear` clear values stored in local `cdk.context.json`. Useful if deployment fails with "resource not found" kind of error.
- `aws s3 sync <YOUR_BUILD_FOLDER> <YOUR_S3_STACK>` (example `aws s3 sync build/ s3://devlearnshowcaseappstack-cbdevwebsitebucketc58c99-p6p2vkvq5olx`) - manual sync your build with bucket (useful  for learninig)
- `aws cloudfront create-invalidation --distribution-id <DISTRIBUTION_ID> --paths "/*"` (example `aws cloudfront create-invalidation --distribution-id E1VYHF5KNCJCK0 --paths "/*"`) - invalidate cloudfront cache (useful for fronted deployment if automatic invalidation isn't provided by github actions)
## How to add new API stack

1. Add API configuration into `./config/dev.yaml`. See `databaseApi` config for example.
2. Deploy API using `cdk deploy -c env=dev dev<NEW_API_STACK_NAME>`

Note: aws service will be named as serviceName+'Service'. I.e. if serviceName is 'scriptureApi', aws service name will be created as 'scriptureApiService'. You will use it in github actions config.

Note: wen you run `cdk deploy -c env=dev dev<NEW_API_STACK_NAME>` , deployment process could hang up and wait for the docker container (if relevant docker container is absent at dockerhub). You can set up relevant git hub action at repository and build the container, then aws deployment process will be continued.

## How to add new Frontend stack

1. Add frontend configuration into `./config/dev.yaml`. See `showcaseApp` config for example.
2. Deploy frontend using `cdk deploy -c env=dev dev<NEW_FRONTEND_STACK_NAME>`. Example for dev env: `npx cdk deploy -c env=dev devDocsStack`

## Workaround for AWS Cloudwatch policy limit

When creating more than 10 backend services, AWS rejects the deployment because of having a limit for resource policies. Workaround:

List all policies:

`aws logs describe-resource-policies`

Delete some policies:

`aws logs delete-resource-policy --policy-name devNotificationsApiStacknotificationsApiFargateTaskDefnotificationsApiContainerLogGroupPolicyB30998C8`

Put a shared policy to grant access for all services:

`aws logs put-resource-policy --policy-name devFargateTaskDefContainerLogGroupPolicyAllServices --policy-document '{ "Version": "2012-10-17", "Statement": [ { "Effect": "Allow", "Principal": { "AWS": "808019037620" }, "Action": ["logs:CreateLogStream", "logs:PutLogEvents"], "Resource": "arn:aws:logs:us-east-2:808019037620:log-group:/ecs/*" } ] }'`
