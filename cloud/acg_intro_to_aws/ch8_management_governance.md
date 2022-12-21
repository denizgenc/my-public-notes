# Introduction
Account management:
- Control Tower: Set up and govern a secure multi-account AWS environment
  - This basically automatically manages IAM Identity Center, AWS Organizations and the AWS Service
    Catalog.
- Organizations
- Budgets

Provisioning:
- CloudFormation
- Service Catalog: Create organize and govern your own curated catalog of AWS products
  - Use CloudFormation to define what exactly people in your account can deploy, or make certain
    configurations easily repeatable and discoverable within the org.
  - e.g. create an AMI that has Wordpress pre-installed with your org's themes and plugins, and then
    write a CloudFormation template that launches an EC2 instance, RDS instance etc with all the
    right configurations, and then put it in the service catalog. Whenever someone needs a new blog
    or whatever, they can find it from the catalog.
- OpsWorks: Use Chef or Puppet to automate stuff
- Marketplace: Find, test and buy software that runs on AWS

Operation:
- CloudWatch
- Config: "continually assesses, audits, and evaluates the configurations and relationships of your
  resources."
- CloudTrail
- Systems Manager: "optimise performance and security while managing a large amount of systems"
  - A bunch of almost unrelated services all bundled together tbh. Most importantly has Parameter
    Store in it
- X-Ray: Analyse and debug production applications.
  - Allows you to follow requests as they move through various services (e.g. Lambda), so you can
    record traces.
  - Build a service map to find issues like latency etc

# CloudFormation
- Templates are like Terraform source files. Can use JSON or YAML.
  - You can also have nested templates - templates that are called by other templates. Like
    Terraform modules.
  - Apparently templates also have "multi-region functionality", which smooths over any
    region-specific configuration you might have to do? Not sure if this is something special or if
    the instructor is referring to things like format strings (e.g. "{aws.region}a")
- The infrastructure that you deploy from a template is called a stack.

# CloudWatch
- You can get metrics from 70+ AWS services
  - Metrics can be visualised and used to set alarms
    - You can define custom metrics as well
  - Alarms can trigger "various things". I guess they connect to SNS or something?

Example of using CloudWatch and CloudWatch logs together:
- You have an EC2 instance that can be SSH'ed into, and want to see if malicious actors are trying
  to gain access to the instance.
  - Send SSH login logs to CloudWatch Logs
  - Create a filter in CloudWatch Logs to search for failed logins
  - Graph the data from the filter
  - Create an alarm based on that graph

# Auto Scaling
Horizontal scaling of EC2 servers (i.e. add more servers under high demand, as opposed to vertical
scaling, which would be upgrading the servers you have [which would come with downtime]).
- You can also auto scale DynamoDB tables and Aurora replicas

There are three parameters:
- Minimum size: the lowest number of instances that *have* to be running.
- Desired capacity: the number of instances you would *prefer* to have running.
  - You can modify the desired capacity on the fly based on CloudWatch metrics (CPU/RAM usage etc)
- Maximum size: the highest number of instances that the scaling group can create.

A load balancer will distribute traffic to the servers in the auto scaling group as the instances
are created and destroyed.
