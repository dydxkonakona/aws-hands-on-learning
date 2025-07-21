# Virtual Private Cloud

## What I Did

- Explore the basic components of a VPC.
<!-- - Deploy a basic VPC with public subnets. -->
- Deploy an Amazon Elastic Compute Cloud (Amazon EC2) instance into a VPC.

### Explore the default VPC

We can see here that the default vpc created by AWS has IPv4 CIDR range of 172.31.0.0/16 which means it has 65,536 ip addresses on this default vpc.

![vpc-explore](./screenshots/vpc/vpc-explore.png)
![vpc-explore2](./screenshots/vpc/vpc-explore2.png)

The default vpc has 3 public subnets in three availability zones. Each subnet has 20 as the prefix of their CIDR range which means they have 4091 ip addresses in each subnet.

![vpc-explore3](./screenshots/vpc/vpc-explore3.png)
![vpc-explore4](./screenshots/vpc/vpc-explore4.png)

This is the internet gateway this is used to allow public subnets to access the internet. It is currently attached to the vpc provided. In a more accurate sense it provides a target in route tables that connects to the internet.

![vpc-explore5](./screenshots/vpc/vpc-explore5.png)

This is the route table of the default vpc provided by AWS. It has two target routes one is local which allows all subnets in the VPC to communicate to one another and an internet gateway route which connects to the internet.

![vpc-explore6](./screenshots/vpc/vpc-explore6.png)

We can view the subnet associations to see which subnets are associated with this route table.

![vpc-explore7](./screenshots/vpc/vpc-explore7.png)

Currently there is none let's try adding a public subnet to it to allow that subnet to access the internet.

![vpc-explore8](./screenshots/vpc/vpc-explore8.png)

### Launching an EC2 instance into a VPC

Let's launch an EC2 instance to the default VPC and to a subnet.

![vpc-ec2](./screenshots/vpc/vpc-ec2.png)

Let's confirm whether the instance is in the subnet.

![vpc-ec2-2](./screenshots/vpc/vpc-ec2-2.png)

As we can see the subnet where we deployed are instance has it's available ip addresses from 4091 gone to 4090 which means the EC2 instance occupied that one ip address. We can also verify it on the instance itself by checking its vpc id and subnet id.

![vpc-ec2-3](./screenshots/vpc/vpc-ec2-3.png)
