# infiniticg-project
This project is intended for demo purposes only.  It is intended for InfinitiCG review/use only. 
Here is the requirements

Then here are a few points about the solution
- This code is wrriten in terraform, so user needs to download and set up terraform.
- Code was built on Linux rhel 7.x , though this should not matter.
- Web tier (auto scaling group for HA and elasticity) uses linux rhel 7.4 and httpd/apache server.
- User needs to edit vars.tfvar with his own vpc and subnet info. If other region that is not us-west-2 is chosen, edit the ec2AMI variable
since ami-s are scoped to regions.  If one leave leaves the region unchanged, then this is not necessary.
Ensure vpc has at least 2 private subnets with routes to a NAT.  Also, ensure vpc has at least 2 public subnets.
ELB is placed in public subnet in the code and autoscaling group launched instances are placed in the private subnets.
One may update ssh-key and use own for testing. ssh to private ec2s using the elb dns name.  This features is only for testing purposes but in prod.  Instead a bastion host is used.
Create credential file and define in it profile and aws id and secret key info for the aws account to be used. One may choose other approaches altogether to passing this info to terraform if he/she wishes to.
- Solution makes use of EFS and auto mounts targets into ec2 so that web-content is separated from web server (resiliancy).  However, efs is not mounted into ec2 (code commented out) because of what appears like a race condition that makes resolving efs dns name fail on first attempt (It succeeds on follow up events).  Therefore, content is loaded into /var/www/html/index.html on ec2
Solution uses ELB.  However, I would use ALB if I had to do another release as ALB is better suited for http traffic.
