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
