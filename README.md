
## Requirements

* AWS Account
* An AWS Identity and Access Management (IAM) user with administrator permissions.
* Access to a remote device, such as a Raspberry Pi
* AWS cli installed locally (version 2.1.5+)
* Ansible cli installed locally  (version 2.10.x+)

## Install Ansible locally

`pip install ansible`

## Create hosts file

You can create a hosts file locally for use with the subsequent steps, or use the more global `/etc/ansible/hosts` file. This project assumes the use of a local hosts file specified using the `-i` argument with the Ansible CLI.

See the following documentation for more information: https://docs.ansible.com/ansible/latest/user_guide/intro_getting_started.html#id4

## Generate SSH keys

Execute this command from the machine where you will be running Ansible, targeting each of the remote hosts you will be targeting with your Ansible playbooks:

`ssh-copy-id <USERNAME>@<IP-ADDRESS>`

See the following for more information: https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md

## Test Ansible commands

Before attempting to run the playbook command in the next step, it is recommended to run a simple remote ping command across your hosts to ensure that your environment is setup correctly:

`ansible all -m ping -i ./hosts -u pi`


## Provision Greengrass V2 on your remote devices using the following playbook

This command will prompt for an `aws_secret_access_key` and `aws_access_key_id` as well as an aws_region and a Thing Name for your Greengrass Core device.

`ansible-playbook install-ggv2-playbook.yml -i ./hosts -u pi`

## Configure your Greengrass IAM Role created by the V2 Installer to allow it to pull Component artifacts from S3

As stated in the documentation, the IAM Role attached to the IoT Role Alias used by your Greengrass Core device does not, by default, have access to download component artifacts from any S3 Bucket. In order to develop and deploy your own custom Greengrass components, you will need to update the `greengrass-component-artifact-policy.json` file by inserting the name of the S3 bucket you will use to host your Greengrass component artifacts.

Then, you will need to create the IAM policy and attach it to your Greengrass IAM role.

See the following documentation for more information: https://docs.aws.amazon.com/greengrass/v2/developerguide/device-service-role.html#device-service-role-access-s3-bucket

## Install Docker for use with Greengrass applications

`ansible-playbook install-docker-playbook.yml -i ./hosts -u pi`
