# William-Sonoma

Necessary steps to go through setting up and creating .conf script using shell scripting and then deploying in CentOS 7 Server:-


1.Connect to your EC2 instance and install the Apache web server.

:$ sudo yum -y install httpd


2.Start the service.

:$ sudo service httpd start


3.Create a mount point

:Since we know that our documentRoot in the /etc/httpd/conf/httpd.conf file points to /apps/hello-http/html (DocumentRoot "/var/www/html")which is for
hello.html program template.So I will need to  mount my Amazon EFS file system on a sub-dir under the document root.


Then, I will be creating a sub-dir    efs-mount-point under  /apps/hello-http/html efs-mount-point.

Next step is to mount an Amazon EFS file system. I need to update the following command by providing your file system ID and AWS region (depending on what region i live currently).

:
$ sudo mount -t nfs -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport file-system-id.efs.aws-region.amazonaws.com:/ /apps/hello-http/html efs-mount-point

Then we will be testing our set up 
a) First I will add a rule in the EC2 instance security group, whichI just created in the Getting Started exercise, to allow HTTP traffic on TCP port 443 from anywhere.

After I add the rule, the EC2 instance security group will have the following inbound rules.

As I already have sample created hello.html file, my next step would be to write a conf script for this in order to generate a configuration which should set up an HTTP Instance at a static web page.

$ echo "<!DOCTYPE html PUBLIC "-//IETF//DTD HTML 2.0//EN">
<HTML>
    <HEAD>
        <TITLE>Hello Williams-Sonoma!</TITLE>
    </HEAD>
    <BODY>
        <H1>Hello Williams-Sonoma!</H1>
    </BODY>
</HTML>" > hello.html   

I can change the directory as follows thereafter:

  
$ cd / apps/hello-http/html/ efs-mount-point


$  sudo mkdir sampledir  
$  sudo chown  ec2-user sampledir
$  sudo chmod -R o+r sampledir
$  cd sampledir 


Now , its time to show this script working on a browser web-page whether its able to successfully deploy or not.We will be further write the URL as follows:


http://EC2-instance-public-DNS/efs-mount-point/sampledir/hello.html

This means that my conf script is able to generate the HTTP Instance of a static webpage delivering as HTTP Status:200 (OK!).
