EC2 CleanUp
===========

Utility to clean up AWS EC2 security groups and keypairs.

- http://abstractvisitorpattern.blogspot.co.uk/2013/03/tidying-up-after-jclouds.html

## About

Removes any unused objects in an EC2 region whose names match a regular expression.

This is particularly useful for cleaning up groups and keys created by jclouds, which have names like `jclouds#0eca9048cc1e47659736c236fbbdb6c2` or `jclouds#03230716fff7412cb7241c78de86e99d#eu-west-1`. These may not be deleted when a program exits, and can accumulate and cause problems with the AWS rate-limit checks. The EC2 management console can show the number of objects in a particular region, using the links below:

- [KeyPairs](https://console.aws.amazon.com/ec2/home?region=eu-west-1#s=KeyPairs)
- [SecurityGroups](https://console.aws.amazon.com/ec2/home?region=eu-west-1#s=SecurityGroups)

## Building

Build the program using Maven:

    % mvn clean install
    ...

This will create an executable Jar file named `ec2cleanup-0.1.0-SNAPSHOT-jar-with-dependencies.jar` containing the code and all its dependencies.

## Usage

Use the program as follows, substituting your EC2 access key and secret key for the `aws-ec2.identity` and `aws-ec2.credential` properties:

    % java -Daws-ec2.identity=XXXXXXXXXXXXXXXXXXXX -Daws-ec2.credential=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX \
        -jar target/ec2cleanup-0.1.0-SNAPSHOT-jar-with-dependencies.jar
    [INFO] Cleaning SecurityGroups and KeyPairs in aws-ec2:eu-west-1 matching 'jclouds#.*'
    [INFO] Found 1649 matching KeyPairs
    [INFO] Deleted 1649 KeyPairs
    [INFO] Found 1508 matching SecurityGroups
    [WARN] Error deleting SecurityGroup 'jclouds#GmqCZ6hx#eu-west-1': There are active instances using
    security group 'jclouds#GmqCZ6hx#eu-west-1'
    [INFO] Deleted 1507 SecurityGroups

The default region is Amazon Europe (`eu-west-1`) which can be changed by setting the first argument. The default regular expression is `jclouds#.*`. You can use a different regular expression by adding it as the second argument, and supplying a region name as the first argument. To delete a specific key pair or security group use the complete name as the regular expression and only that object will be deleted.

    % java -Daws-ec2.identity=XXXXXXXXXXXXXXXXXXXX -Daws-ec2.credential=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX \
        -jar target/ec2cleanup-0.1.0-SNAPSHOT-jar-with-dependencies.jar us-east-1 jclouds#grkvlt#.*
 
**NOTE** It is not possible to delete a security group if it is being used by an active instance, but because key pairs are only needed when starting an instance they can always be deleted.

## License

Copyright 2013 by Andrew Kennedy

> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:
> 
> The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.
> 
> THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
