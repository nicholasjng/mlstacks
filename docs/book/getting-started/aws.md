# Quickstart on AWS

This quickstart will guide you through deploying a simple stack on AWS using
`mlstacks`. We'll be deploying a simple S3 bucket. This is as simple and quick
an example of how `mlstacks` works as it gets.

## Prerequisites

First, install the `mlstacks` CLI:

```bash
pip install mlstacks
```

You'll need an active AWS account to get started. You will also need sufficient
permissions to be able to create and destroy resources.

If you don't have
[Terraform](https://learn.hashicorp.com/tutorials/terraform/install-cli#install-terraform)
or [Helm](https://helm.sh/docs/intro/install/#from-script) installed, you should
also install them.

## Defining our stack

Then, create a file called `quickstart_stack.yaml` wherever you have access to
the `mlstacks` tool. In this file, add the following:

```yaml
spec_version: 1
spec_type: stack
name: "quickstart_stack"
provider: aws
default_region: "eu-north-1"
default_tags:
  deployed-by: "mlstacks"
components:
  - simple_component_s3.yaml
```

This defines our stack using the `mlstacks` specification. We'll now define the
component that we want to deploy in a separate file called
`simple_component_s3.yaml`:

```yaml
spec_version: 1
spec_type: component
component_type: "artifact_store"
component_flavor: "s3"
name: "quickstart_s3_bucket"
provider: aws
metadata:
  config:
    bucket_name: "quickstart_s3_bucket"
  tags:
    deployed-by: "mlstacks"
  region: "eu-north-1"
```

## Deploying our stack

Now, we can deploy our stack using the `mlstacks` CLI:

```bash
mlstacks deploy -f quickstart_stack.yaml
```

This will deploy our stack to AWS. It will also deploy/provision an S3 bucket
(beginning with `zenml-mlstacks-remote-state` by default) which will be used as
a remote state store and backend for your Terraform assets. This will happen
first before the deployment of your stack. You can now check your AWS console to
see that the stack (and remote state bucket) has been deployed.

## Get stack outputs

You can get the outputs of your stack using the `mlstacks` CLI:

```bash
mlstacks output -f quickstart_stack.yaml
```

This will print out the outputs of your stack, which you can use in your
pipelines.

## Destroying our stack

Finally, we can destroy our stack (and the remote state S3 bucket) using the
`mlstacks` CLI:

```bash
mlstacks destroy -f quickstart_stack.yaml
```

## What next?

You can now try adding more components and deploying them to your cloud
provider. You can also try deploying your stack to a different cloud provider.

Good luck! And if you have any questions, feel free to
[reach out to us on Slack](https://www.zenml.io/slack-invite)
