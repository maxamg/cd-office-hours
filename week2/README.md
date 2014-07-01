WEEK 2
======

### IMPORTANT:
Some of the directory paths are relative. The instructions assume
you have all github repositories cloned in the same parent directory.
Adjust as necessary.


1. Download a Hello World app project:

   ```
   git clone https://github.com/maxamg/dropwizard-helloworld
   ```
1. Ensure you have maven installed

   ```
   brew install maven
   ```
   
1. Build the java app

   ```
   cd dropwizard-helloworld
   mvn clean package
   ```
   
1. Stage the build artifacts in your week2 directory

   ```
   cd ../cd-office-hours/week2
   mkdir artifacts
   cp ../../dropwizard-helloworld/target/dropwizard-helloworld-0.0.1-SNAPSHOT.jar artifacts/
   cp ../../dropwizard-helloworld/config/dev_config.yml artifacts/
   ```

1. Test the app on your host OS

   ```
   java -jar artifacts/dropwizard-helloworld-0.0.1-SNAPSHOT.jar server artifacts/dev_config.yml
   ```
   Navigate to your browser and hit [http://localhost:8080/helloworld](http://localhost:8080/helloworld)
   
1. Ctrl-C the process to kill the app

1. Create an Ansible play to sync the dropwizard binaries to the vagrant host and start the java process


   See [https://github.com/maxamg/cd-office-hours/blob/master/week2/deploy_java_app.yml](https://github.com/maxamg/cd-office-hours/blob/master/week2/deploy_java_app.yml)

1. Create a simple upstart script to daemonize the java process


   See [https://github.com/maxamg/cd-office-hours/blob/master/week2/dropwizard_upstart.j2](https://github.com/maxamg/cd-office-hours/blob/master/week2/dropwizard_upstart.j2)

1. To execute the playbook:

   ```
   vagrant up
   ansible-playbook -vvv -i inventory --user vagrant --private-key=~/.vagrant.d/insecure_private_key deploy_java_app.yml
   ```

1. Ensure you can hit the endpoint [http://192.168.33.20:8080/helloworld](http://192.168.33.20:8080/helloworld)

1. Destroy your vagrant VM to save space (we'll create more in next weeks session):

   ```
   vagrant destroy -f
   ```
