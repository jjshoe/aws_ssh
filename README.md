# AWS SSH
Easily SSH to Amazon AWS EC2 instances. 

## About

How often do you login into your AWS console to find an IP address or domain name that points to an instance? AWS SSH lets you specify simple or advanced filters to let you connect to your instances quickly. If multiple instances match you can review all the tags associated with an instances, and pick which one to connect to.

## Installation

Make sure you have the required modules, and you should be good to go. 

```
sudo pip install configargparse boto tabulate
```

## Configuration
The first thing you need to do is configure boto, you can do this by adding the following to ```~/.boto```:

```
[Credentials]
aws_access_key_id = <your_access_key_here>
aws_secret_access_key = <your_secret_key_here>
```

If you want to SSH as someone other than your login user, you can pass -u, --user, or you can add the following to ```~/.aws_ssh```:
```
user = bob_barker
```

## Usage
```
usage: aws_ssh.py [-h] [-a FILTERS] -u SSH_USER [search]
```

## Examples

##### Basic:

```
[jjshoe@desktop aws_ssh (master %)] ./aws_ssh.py *web*
+-----+----------------+----------------+-----------+-----------------+
|   # | IP Address     | Name           | Region    | environmentName |
+=====+================+============================+=================+
|   1 | 127.0.0.1      | prod_web_01    | us-east-1 | prod            |
+-----+----------------+----------------+-----------+-----------------+
|   2 | 127.0.0.2      | prod_web_02    | us-west-2 | prod            | 
+-----+----------------+----------------+-----------+-----------------+

Enter the number of the instance to connect to: 1
Connecting to jjshoe@127.0.0.1
Last login: Mon Mar 23 17:11:46 2015 from 127.0.0.3
jjshoe@prod_web_01:~$
```

#####Advanced:

```
[jjshoe@desktop aws_ssh (master *)]$ ./aws_ssh.py -a tag:environmentName=prod -a tag:serverRole=www
+-----+------------+--------+-----------+-----------------+------------+
|   # | IP Address | Name   | Region    | environmentName | serverRole |
+=====+============+========+===========+=================+============+
|   1 | 127.0.0.1  | www_01 | us-east-1 | prod            | www        |
+-----+------------+--------+-----------+-----------------+------------+
|   2 | 127.0.0.2  | www_02 | us-east-1 | prod            | www        |
+-----+------------+--------+-----------+-----------------+------------+

Enter the number of the instance to connect to: 2
Connecting to jjshoe@127.0.0.2
Last login: Mon Mar 23 17:14:18 2015 from 127.0.0.3
jjshoe@www_01:~$
```

You can specify as many advanced filters as you want by chaining -a over and over again. To see a list of what all can be passed in visit http://docs.aws.amazon.com/AWSEC2/latest/CommandLineReference/ApiReference-cmd-DescribeInstances.html and search for ```Supported Filters```.

