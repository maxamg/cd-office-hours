WEEK 2
======

1. Download a Hello World app project:

   ```
   git clone https://github.com/sullis/dropwizard-helloworld
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
   
1. Test the app on your host OS

   ```
   java -jar target/dropwizard-helloworld-0.0.1-SNAPSHOT.jar server config/dev_config.yml
   ```
   Navigate to your browser and hit [http://localhost:8080/helloworld](http://localhost:8080/helloworld)
   
1. Create an Ansible play to sync the dropwizard binaries to the vagrant host and start the java process

   ```
   TBC
   ```
1. Create a simple upstart script to daemonize the java process

   ```
   TBC
   ```
