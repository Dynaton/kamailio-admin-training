# Kamailio Admin Training: Using Kamailio as a Registrar for FusionPBX

The focus on the training is provide attendees with an overall understanding of Kamailio 5.3 with hands on labs
that gives attendees experience with configuing Kamailio as a Registrar Server for FusionPBX

This repo contains Terraform scripts that will spin up a server ready to install Kamailio 5.3 and a server instance with 
FusionPBX already installed ready to be used in the class.

We also provide the completed Kamailio configurations for the different labs.  The class material and labs are not provided in this repo,
but you can get more information about the class from here: http://dopensource.com/kamailio-training/  

The goal of this repo is to empower more people to learn about Terraform, which is a tool to realize the vision of Infrastructure as Code (IaC) with a focus on VoIP 
This means that you can use code to provision (aka setup) new infrastrature and destroy it once you are done using it.  You can do this over and over again.
This is very useful when you are setting up lab environments for classes or workshops.  So, we hope this will spark interest and will cause people to contribute to
make it better and add other Cloud Providers.  

## Instructions for Provisioning the Environment

1. Setup a Digital Ocean account and obtain an API token.  Save the token somewhere safe.  Treat the token like a password.

2. Download Terraform from https://www.terraform.io/downloads.html

3. Create a new ssh-key that can be used to connect to the instances

```
ssh-keygen -f <file location to place the newly generated keypair>
```

4. Upload the public key to Digtial Ocean.  Terraform will use the private key to install software on the VM's during the provisioning phase and you 
can connect to the VM's via SSH using the private key. 

5. Execute the following:

```
cd terraform
terraform init
terraform apply -var do_token="Digital Ocean Token" -var pvt_key="location of the key"
```

I usually put the Digital Ocean Token in an environment variable and then execute Terraform like this:

Note: the token below is not real.  Just trying to show an example :-/

```
DIGITALOCEAN_TOKEN="34950239605awjjgwqj2tlgljl2rntkgnnlnrtl2n"
cd terraform
terraform init
terraform apply -var do_token=$DIGITALOCEAN_TOKEN -var pvt_key="~/.ssh/dopensource-training" 
```

6. Enter "yes" to the start the provisioning process


Note, by default it will deploy one lab environment, which includes 2 Virtual Machines with Debian 9 Stretch installed.  One Virtual Machine will be install with 
all of the required libraries needed to install Kamailio from source.  The other Virtual Machine will install FusionPBX automatically during the provisioning process.  

Set the num_of_envivronments variable if you want to provision multiple servers.  The apply statement will look like this:

```
terraform -var do_token=$DIGITALOCEAN_TOKEN -var pvt_key="~/.ssh/dopensource-training" -var num_of_environments=<number goes here> apply
```

## Notes about the labs

Each directory represents a lab

Each directory contains commands for the labs or a completed Kamailio configuration file for that lab

You can make the Kamailio file active by changing into one of the lab directories and activing the following command

```
ln -s $PWD/kamailio.cfg /usr/local/etc/kamailio/kamailio.cf
```

Note, Kamailio will be installed in the /usr/local directory
