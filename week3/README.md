WEEK 3
======

Some of the directory paths are relative. The instructions assume you have all github repositories cloned in the same parent directory.
Adjust as necessary.

In this session we aim to create two VM's: the first is based on a simple app-server VM that will serve as our deployment target, the second will be our Go CD server from which we'll be building and deploying the simple hello world application used in week2.

For these two servers I've kept things simple and started with two separate playbooks:

    deploy_app_server.yml
    deploy_cd_server.yml
    

I've used the recommended role/task layout as documented on the [Ansible site](http://docs.ansible.com/playbooks_best_practices.html#directory-layout).

1. Create a roles directory and import the existing Ansible GOCD role from the ThoughtWorks organization: [https://github.com/ThoughtWorksInc/ansible-gocd](https://github.com/ThoughtWorksInc/ansible-gocd)

   ```
   mkdir roles
   git clone https://github.com/ThoughtWorksInc/ansible-gocd
   ```

1. Define your servers in the ansible inventory, one for app-server and one for the cd-server

1. Update the `deploy_cd_server.yml` to call the desired role. E.g.

    ```
    - hosts: CDServer
      tags: server
      roles:
       - ansible-gocd
       - build_deploy_tools
    ```
    I've created a separate role to include some of the build/deployment prerequisite tools like Maven, Git, Ansible

1. Start the CD server Vagrant instance:
    
    ```
    vagrant up go-server
    ```

1. Provision the CD server:

	```
	
	```


1. Destroy your vagrant VM to save space (we'll create more in next weeks session):

   ```
   vagrant destroy -f
   ```
