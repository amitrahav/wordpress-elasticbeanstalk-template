# Wordpress ElasticBeanstalk Template
> this is ready for deploy Aws ElasticBeanstalk template

## Dependencies

1. AWS account.

2. Create multi-container Beanstalk App (this repo configured to work with codeBuild).

3. Add inline policy (as shown at logs part) to beanstalk IAM profile.

## Optional

1. Installing [eb cli](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb-cli3-install.html)

## How to use

```shell
mkdir ${name-of-your-project}
cd ${name-of-your-project}

git clone   https://github.com/amitrahav/wordpress-elasticbeanstalk-template.git ./

eb init
```

You Should add php-fpm docker url to Dockerrun.aws.json (line 24). 

I Use [Wp-Engine](https://github.com/amitrahav/WP-Slim-Container) as php-fpm container.

### Folders Structure

```javascript
.ebextensions // elasticbeanstalk configuration for sharing upload folder and adding nginx + php into cloudwatch log stream

.elastickbeanstalk //for using eb cli tool

config // nginx and php-fpm configuration

site // app files

.gitignore //ready to use with bedrock from roots.io

buidspec.yml // code build instructions

Dockerrun.aws.json // multi container port binding and instances configuration

README.md // here i am
```

### App files

It's recommended to use [bedrock](https://roots.io/bedrock/).

```shell
cd site
composer create-project roots/bedrock ./
```

## How Does It work

1. dockerBuild 

## Logs

Cloudwatch integration can only work with [this](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/AWSHowTo.cloudwatchlogs.html#AWSHowTo.cloudwatchlogs.loggroups) IAM instance profile:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:GetLogEvents",
        "logs:PutLogEvents",
        "logs:DescribeLogGroups",
        "logs:DescribeLogStreams",
        "logs:PutRetentionPolicy"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
```
