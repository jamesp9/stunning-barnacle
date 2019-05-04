# Tech test
Uses AWS CloudFormation to create two seperate environments:
**training** and/or **prod** .

**training**
A VPC with one single public subnet with an EC2 instance deployed directly  
into it.  The instance has Tomcat installed and displays the sample Tomcat  
"hello world" app.  The ".war" file in this case is retreived from the Apache  
site, which could be any URL.

**prod**
A VPC with two public subnets and two private subnets.
An application load balancer is attached to the public subnets and is  
associated with an auto scaling group. Currently the desired instance count  
is set to two with the idea to increase this number as demand increases.



# Requirements
* `aws` command installed
* `default` profile in `~/.aws` that has administrative privileges

# Instructions
```
git clone https://github.com/jamesp9/stunning-barnacle.git
cd stunning-barnacle
```
## Create environements
The `create_stack` script will create an SSH keypair and import it into EC2.   
By default the stack will build in the Sydney region, this can be overidden by   
adding the `REGION` environment variable before the `create_stack` command e.g.   
`REGION="us-west-2"`

```
ENV="training" ./create_stack
```

```
ENV="prod" ./create_stack
```

## Delete environements when finished
```
ENV="training" ./delete_stack
```
```
ENV="prod" ./delete_stack
```


## AWS Console
Alternatively the templates can be setup from the AWS Console.
Goto: `CloudFormation` 
* click `Create Stack`
* choose `Upload a template to Amazon S3` 
* `Browse..` to either `environment_prod.json` or `environment_training.json`
* click `Next`
* Enter a stack name and adjust the Parameters:  
`KeyName` with an SSH key you already have
`SSHLocation` adjust to your public IP if you want to SSH into the instance
* `Next`
* `Next`
* `Create`
  
