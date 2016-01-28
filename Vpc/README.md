These templates will create a basic VPC in either 2 or 3 availability zones.

These templates are based on VPC design recommendations from 
[https://medium.com/aws-activate-startup-blog/practical-vpc-design-8412e1a18dcc](https://medium.com/aws-activate-startup-blog/practical-vpc-design-8412e1a18dcc)

## How to Use These Templates

1. Decide if you want 2 or 3 availability zones and choose the appropriate template. Some regions only support
2 AZs, so be aware of that.
2. Decide which AZs you want to use. Not all AZs can be used by all AWS accounts.
3. Decide your VPC CIDR block. These templates will create a VPC based on a /16 CIDR block,
for example, 10.100.0.0/16. Make note of the first 2 digits, eg. 10.100, you'll need them as input parameters
to the template.
4. Select an appropriate NAT AMI image.
5. Create a Key Pair and download it's PEM file. This will be used for the NAT.
6. Decide on a name for your VPC (such as 'sample' or 'my-application'). This will be used on the Name tag of
various resources that get created.
7. Create a new stack based on the selected template and information collected from the previous steps.

## Resources

The following resources are ceated:

* 1 VPC based on a /16 CIDR block
* 1 Internet Gateway
* NATs, one per AZ to help with HA
* 1 security group for the NAT:
 * HTTP (port 80) internal-to-external
 * HTTPS (port 443) internal-to-external
 * NTP (port 123) internal-to-external
* 1 IAM role for the NAT instances
* 4 subnets per AZ:
 * 2 public subnets: general and ELB
 * 2 private subnets: general and ELB
* Route tables:
 * 1 Public route table
 * Private route tables, one per AZ

Separate general and ELB subnets are created because some services, such as AWS Elastic Beanstalk, want
ELBs to be in different subnets from the associated EC2 instances.

## Notes

* These 2 templates are compatible with each other. So if you want to change your VPC from 2 to 3 AZs later
(or 3 to 2), you can simply switch templates.
* Once AWS NAT Gateways are available in CloudFormation, the NAT instances will be replaced with that.
