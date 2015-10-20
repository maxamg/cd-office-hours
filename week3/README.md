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
   $ mkdir roles
   $ git clone https://github.com/ThoughtWorksInc/ansible-gocd
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
    I've created a separate role to include some of the build/deployment prerequisite tools like Maven, Git, Ansible, and importantly the same SSH private key we've been using. This will enable the go user (running the go agent processes) to deploy to the app-server VM via password-less SSH.

1. Start the CD server Vagrant instance:
    
    ```
    $ vagrant up go-server
    ```

1. Provision the CD server. From your host machine:

    ```
	$ ansible-playbook -i inventory -u vagrant --private-key ~/.vagrant.d/insecure_private_key deploy_cd_server.yml
	```
    Note: There are several setups to chose from for GOCD. The ansible-gocd module has been set up such that agents and server can be installed on the same box, or separate boxes. Use `--tags server` or `--tags agent` with the ansible-playbook command.
    
1. Ensure the go server has started by pointing your browser at [http://192.168.33.31:8153/go](http://192.168.33.31:8153/go)

1. Spin up the app-server VM ready for a deployment:

   ```
   $ vagrant up app-server
   ```
   Note: no ansible has been run on the app-server at this point. We'll aim to deploy this box via Go CD.
   
1. Go has an XML config backbone, conveniently in one file that describes all pipelines. We need to set up a build and deployment pipeline. An example can be found below the `sample_go_config` directory. copy it to the go-server:

   ```
   $ scp -i ~/.vagrant.d/insecure_private_key sample_go_config/cruise-config.xml vagrant@192.168.33.31:/tmp

   $ vagrant ssh go-server
   $ sudo -i -u go
   $ cp /tmp/cruise-config.xml /etc/go/
   $ service go-server restart
   ```

1. Wait 30 seconds or so for the server to come back up and refresh your browser. You should now see 2 pipelines.

1. Kick off a build by clicking the play button.

1. When the build has completed, kick off the deployment.

1. Confirm the app has been deployed by hitting [http://192.168.33.30:8080/helloworld](http://192.168.33.30:8080/helloworld)

1. Destroy your vagrant VM to save space (we'll create more in next weeks session):

   ```
   $ vagrant destroy -f
   ```
