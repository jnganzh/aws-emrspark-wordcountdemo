# AWS Elastic Map Reduce Spark Demo Wordcount

Code referenced from https://github.com/Aliga8or/csds-spark-emr

This was a good tutorial to get started with AWS EMR spark, however it is slightly outdated and I ran into a couple of issues, I hope this tutorial will help other people avoid the mistakes I made.

## Create an EMR cluster

1. Open up your AWS console and go to EMR.
2. Create a cluster. You should see this page.

![Alt text](http://full/path/to/img.jpg "EMR Cluster Creation Page")

As we are going to be using spark in this tutorial, under Software Configuration -> Applications -> Select Spark.

![Alt text](http://full/path/to/img.jpg "EMR Cluster Creation Page")

Under Security and Access, click "Learn how to create an EC2 keypair" and follow the instructions. Make sure you create this keypair first and select it when you create the cluster if not you will not be able to SSH into the master node.

3. The cluster takes around 5-10 minutes to start.

## Configure S3 bucket

1. Create a S3 bucket, under Block Public Access settings for this bucket, uncheck the block all public access.

![Alt text](http://full/path/to/img.jpg "EMR Cluster Creation Page")

3. Upload a file .txt file into this bucket with some words in it, you can use the input.txt file located in this repo.
4. Before uploading the file scroll to ACL control list, check the box for public access for the objects.

## Configure EC2 inbound traffic

1. Navigate to EC2 by using the search function
2. Under Network & Security select Security Groups
3. Select the group named "ElasticMapReduce-master", edit the Inbound rules.
4. Add a new rule, for type select SSH and My IP as the source.

## Running the Wordcount application.

1. Navigate to EMR through the search function
2. Click on "Connect to the Master Node using SSH". Some instructions will pop up, you will need the keypair you generated earlier.

![Alt text](http://full/path/to/img.jpg "EMR Cluster Creation Page")

3. Type "vi wordcount.py" into the console
4. Copy and paste the code from the wordcount.py file in this repo into the vi window. Change the s3 url in line 16 of the code. Make sure your url starts with "s3://". 
It can be found when you navigate to your S3 bucket and clicking on the .txt file you uploaded earlier.
5. Save it and exit by pressing ESC and typing ":wq" and pressing enter
6. Type "spark-submit wordcount.py | tee output.txt" into the console
7. An output file will be generated, you can see the contents of the file by typing "cat output.txt"

It should look like this:
Launch App..
Initiating main..
Counting words in  s3://spark-emr-wordcount/emr-wordcount-pair/input.txt
There: 1
have: 3
been: 1
many: 1
attempts: 1
to: 2
create: 1
digital: 1
money: 2
in: 3
the: 4
past,: 1
but: 1
they: 2
always: 1
failed.: 1
The: 1
prevailing: 1
issue: 1
is: 3
trust.: 1
If: 1
someone: 2

8. You can copy your s3 bucket by using the command "aws s3 cp output.txt s3://my_bucket/my_folder/"
9. Remember to terminate the EMR cluster when you are done.
