# serverless-toggle-vpc-example

An example on how to toggle the VPC configuration per stage such as disable Vpc configuration for `dev`.

## Installation

Install node modules

```sh
npm install serverless -g
npm install
```

Decrypt the secrets file to get example config values

```sh
sls decrypt --stage prod --password '12345678'
```

NOTE: for your real project, DO NOT share the password on the source control

## Configuration

You can configure Vpc per stage by using the following configuration:

```yml
custom:
  vpc:
    dev:
      # If you don't want to run Lambda under a Vpc, then just give it an empty array
      securityGroupIds: []
      subnetIds: []

    # You can add more stages here

    prod:
      securityGroupIds:
        Fn::Split:
          - ','
          - ${file(secrets.${self:provider.stage}.yml):VPC_SECURITY_GROUP_IDS}
      subnetIds:
        Fn::Split:
          - ','
          - ${file(secrets.${self:provider.stage}.yml):VPC_SUBNET_IDS}
```

## CLI

Deploy to `dev` stage without Vpc

```sh
sls deploy
```

Deploy to `prod` stage will add Vpc configuration to your Lambda functions

```sh
sls deploy --stage prod
```
