# AWS CloudFormation Template Files

## 1. Dependency

 - [awscli](https://docs.aws.amazon.com/streams/latest/dev/kinesis-tutorial-cli-installation.html)

## 2. How to use

Dependent packages are installed automatically.

Create AWS Virtual Private Cloud (VPC).

```
aws create-stack --stack-name <value> --template-url <value>

--stack-name (string)
   The  name that is associated with the stack. The name must be unique
   in the region in which you are creating the stack.

   NOTE:
       A stack name can contain only alphanumeric characters (case sen-
       sitive)  and hyphens. It must start with an alphabetic character
       and cannot be longer than 128 characters.

--template-url (string)
   Location of file containing the template body. The URL must point to
   a template (max size: 460,800 bytes) that is located in an Amazon S3
   bucket.  For more information, go to the Template Anatomy in the AWS
   CloudFormation User Guide.

   Conditional: You must specify either the template-body or  the  tem-
   plate-url parameter, but not both.
```

Delete VPC.

```
aws delete-stack --stack-name <value>
```


## 3. VPC List

 - Genomon Cloud (genomon-vpc.yaml)

![document](./img/genomon-cloud.PNG)

