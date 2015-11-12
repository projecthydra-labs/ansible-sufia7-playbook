This Ansible project was created by Data Curation Experts for the Chemical Heritage Foundation. It builds a production-style Hydra head on an Ubuntu 14.04 Amazon EC2 instance.

## Prerequisites
[Ansible](http://docs.ansible.com/intro_installation.html) 1.9 or above.

## AWS / EC2 setup
TODO

## Execution
To create, set up an ec2 instance:

1. Copy group_vars/sample_all to group_vars/all.
2. Add your organization's AWS credentials there.
3. Add/change any other variables you wish to override.
4. Consider protecting group_vars/all with ansible-vault.
5. run `ansible-playbook -i hosts --private-key /path/to/your/keypair.pem create_ec2.yml`
   * Optional: if you encrypted your variables with ansible-vault, add `--ask-vault-pass`
   * Optional: The scripts will by default launch an instance tagged 'staging'. If you want a different tag, override the 'aws_tag' variable either in a file (i.e. group_vars/all) or directly from the command line by adding: `--extra-vars "aws_tag=production"`
6. Note that this playbook is NOT idempotent -- it creates a new instance on AWS each time it is run.

There's another playbook called configure.yml. It's included by create_ec2 but can also be run separately. This playbook IS idempotent, so it can be used to change configuration at will. Also, AWS-related tasks have been removed (let us know if you run into anything!) so it should be more generalizable. Note that to run this playbook you must specify a hosts variable (see comment in configure.yml).

## Deployment
This project expects your code to be deployed with [Capistrano](http://capistranorb.com/). In your Hydra head (the codebase you're deploying), set the Capistrano `:deploy_to` directory to match the housekeeping role's `project_name` variable. If you use the default value for `project_name` in the housekeeping role, you should use 
```
set :deploy_to, '/opt/sufia-project'
```
in `config/deploy.rb` and/or in `config/deploy/<yourenv>.rb`  

## Vagrant
[Vagrant](http://docs.vagrantup.com/v2/)

### A production-like vagrant box
To set up a production-like Vagrant box (for staging, troubleshooting) for you project:

1. Create a Vagrant file in your project
  * (see sample_Vagrantfile for ideas)
  * Be sure to point to the vagrant_staging.yml file, which skips aws-related roles
2. Clone this repository alongside your project
3. cd into your project and run `vagrant up`
4. TODO: deploy your capistrano project to your vagrant box. Haven't tried this yet.
