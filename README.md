# EC2 Image Builder

learn [EC2 Image Builder](https://docs.aws.amazon.com/imagebuilder/latest/userguide/what-is-image-builder.html)

> EC2 Image Builder is a fully managed AWS service that makes it easier to automate the creation, management, and deployment of customized, secure, and up-to-date server images that are pre-installed and pre-configured with software and settings to meet specific IT standards.

> AWS Systems Manager Agent (SSM Agent) is Amazon software that runs on Amazon Elastic Compute Cloud (Amazon EC2) instances, edge devices, and on-premises servers and virtual machines (VMs). SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources. The agent processes requests from the Systems Manager service in the AWS Cloud, and then runs them as specified in the request. SSM Agent then sends status and execution information back to the Systems Manager service by using the Amazon Message Delivery Service (service prefix: ec2messages).

---

## Process

1. select base AMI (e.g. amazon linux 2, a custom AMI).  must have SSM agent installed.
2. select IAM role that is used as instance profile for EC2 build process and test process
3. select amazon provided build components or specify your own build components
  - amazon provided e.g. correto
  - custom - defined via yaml with steps.  e.g. step types - bash, executable
4. select amazon provided tests or specify own custom tests
4. specify build schedule - manual, schedule builder, CRON 
5. specify which regions to publish the resulting ami to

---

## AWS Systems Manager Agent (SSM Agent)

> AWS Systems Manager Agent (SSM Agent) is Amazon software that runs on Amazon Elastic Compute Cloud (Amazon EC2) instances, edge devices, and on-premises servers and virtual machines (VMs). SSM Agent makes it possible for Systems Manager to update, manage, and configure these resources. The agent processes requests from the Systems Manager service in the AWS Cloud, and then runs them as specified in the request. SSM Agent then sends status and execution information back to the Systems Manager service by using the Amazon Message Delivery Service (service prefix: ec2messages).

---

## [AWS Task Orchestrator and Executor (AWSTOE) component manager](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-component-manager.html)

> application to orchestrate complex workflows, modify system configurations, and test your systems without writing code. This application uses a declarative document schema. Because it is a standalone application, it does not require additional server setup. It can run on any cloud infrastructure and on premises.

* single golang binary that can validate and run AWSTOE components documents (yaml)

**`hello-world-linux.yml`**

example AWSTOE components document

```yaml
name: Hello World
description: This is hello world testing document for Linux.
schemaVersion: 1.0
phases:
  - name: build
    steps:
      - name: HelloWorldStep
        action: ExecuteBash
        inputs:
          commands:
            - echo 'Hello World from the build phase.'
  - name: validate
    steps:
      - name: HelloWorldStep
        action: ExecuteBash
        inputs:
          commands:
            - echo 'Hello World from the validate phase.'
  - name: test
    steps:
      - name: HelloWorldStep
        action: ExecuteBash
        inputs:
          commands:
            - echo 'Hello World from the test phase.'
```

see [Get started with AWSTOE](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-get-started.html)

```sh
# validate
awstoe validate --documents hello-world-linux.yml

# run all phases
awstoe run --documents hello-world-linux.yml

# run single phase
awstoe run --documents hello-world-linux.yml --phases build
```

---

## Resources

- [EC2 Image Builder](https://docs.aws.amazon.com/imagebuilder/latest/userguide/what-is-image-builder.html)
- [Youtube - AWS re:Invent 2019: NEW LAUNCH! EC2 Image Builder: Virtual machine images made easy (CMP214-R1)](https://www.youtube.com/watch?v=9XFuRq0R8nk)
- [AWS Task Orchestrator and Executor component manager](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-component-manager.html) - runs image builder and codebuild yaml files
- [Get started with AWSTOE](https://docs.aws.amazon.com/imagebuilder/latest/userguide/toe-get-started.html)