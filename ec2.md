# Elastic Cloud Compute

This hands-on will walk you through how I launched a web server using Amazon EC2

## What I did

- Launch an EC2 instance with termination protection turned on.
- Monitor my EC2 instance.
- Modify the security group that my web server is using to allow HTTP access.
- Connect to my EC2 instance through HTTP and SSH.
- Connect to my EC2 instance using AWS Systems Manager Fleet Manager.
- Manage the state of an EC2 instance.
- Change the EC2 instance type.
- Test termination protection.
- Explore Amazon EC2 limits.

### Launching an EC2 Instance

I will name my EC2 instance my-demo-web-server and I will add tags to this instance for better categorization.

![ec2-launch](./screenshots/ec2/ec2-launch.png)

I will pick Amazon Linux for our AMI which basically means Linux will be the OS of our EC2 instance.

![ec2-launch2](./screenshots/ec2/ec2-launch2.png)

Instance type will be set to t2.micro and we will have the key pair EC2-my-demo-web-server, this key pair will allow us to SSH into our EC2 instance.

![ec2-launch3](./screenshots/ec2/ec2-launch3.png)

I will deploy the EC2 Instance in a default vpc determined by AWS and then attach launch-wizard-1 security group. This security group has inbound rules that allows http and shh access into our EC2 instances.

![ec2-launch4](./screenshots/ec2/ec2-launch4.png)

Termination protection is also turned on to preven accidental termination of EC2 Instance. Default EBS Volume is assigned to this also.

![ec2-launch5](./screenshots/ec2/ec2-launch5.png)

Lastly we will setup our EC2 user data using this code.

```sh
#!/bin/bash
# Use this for your user data (script from top to bottom)
# install httpd (Linux 2 version)
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo "<h1>Hello World from $(hostname -f)</h1>" > /var/www/html/index.html
```

This code will run when we launch our EC2 instance and it will only run at launch. It will install necessary software and display a hello world message. And now we launched the EC2 instance and we can view it now.

![ec2-launch6](./screenshots/ec2/ec2-launch6.png)

### Monitor Instance

It seems that both the System reachability and Instance reachability checks have passed.

![ec2-monitor](./screenshots/ec2/ec2-monitor.png)

On Monitoring tab it displays the Amazon Cloudwatch metrics for the instance such as CPU utilization and network usage.

![ec2-monitor2](./screenshots/ec2/ec2-monitor2.png)

I will check the system logs of the instance. We can see here the messages on the console output of the instance such as when the software packages has been installed as instructed in our EC2 user data. We can troubleshoot problems in the instance here so it's a good practice to check and read system logs.

![ec2-monitor3](./screenshots/ec2/ec2-monitor3.png)

I also have here what the EC2 instance look like if a screen were attached to it.

![ec2-monitor4](./screenshots/ec2/ec2-monitor4.png)

### Accessing the Web Server

I will access now the EC2 instance via http and see how it looks like.

![ec2-access](./screenshots/ec2/ec2-access.png)

I used the public ipv4 given by the AWS to access it (52.221.214.156).

Now I will try to connect to it via SSH.

![ec2-access2](./screenshots/ec2/ec2-access2.png)

Both of these operations are possible because EC2 has a firewall that has inbound rules that allows traffic from http and ssh from anywhere let's see what happens if we remove the inbound rules that allow http access into the EC2 instance. 

![ec2-access3](./screenshots/ec2/ec2-access3.png)

This will be the new inbound rules for launch-wizard-1 security group now let's try accessing the EC2 instance via http.

![ec2-access4](./screenshots/ec2/ec2-access4.png)

As we can see we cannot access it now via http using the public ipv4 address.
Now let's try to connect to the EC2 instance using using AWS Systems Manager Fleet Manager.With the Fleet Manager capability of AWS Systems Manager, you can remotely manage and configure your managed nodes. A managed node is any machine configured for Systems Manager.

![ec2-access5](./screenshots/ec2/ec2-access5.png)

![ec2-access6](./screenshots/ec2/ec2-access6.png)

### Resizing the Instance

If resizing is ever needed for the instance we can do it by first stopping the instance. After stopping it we can now resize in the instance settings.

![ec2-resize](./screenshots/ec2/ec2-resize.png)

![ec2-resize2](./screenshots/ec2/ec2-resize2.png)

Let's resize to t2.nano and apply the changes and let's start the instance again.

![ec2-resize3](./screenshots/ec2/ec2-resize3.png)

It has successfully resized the instance family of the instance is now t2.nano.

### Testing termination protection

I will try terminating the instance and see what happens.

![ec2-terminate](./screenshots/ec2/ec2-terminate.png)

It didn't work because termination protection is turned on, this is good because it will prevent accidental termination of EC2 instances. Now let's turn it off and then terminate it for good.

![ec2-terminate2](./screenshots/ec2/ec2-terminate2.png)

![ec2-terminate3](./screenshots/ec2/ec2-terminate3.png)

![ec2-terminate4](./screenshots/ec2/ec2-terminate4.png)

With termination protection turned off the EC2 instance is now terminated.
