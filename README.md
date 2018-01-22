# ansible-devops
This repo builds a server that will have the follwing software:
* Ansible, for installing and configuring software on other servers.
* Jenkins, for continuous project builds and deployment.
* Git, for managing programs, scripts, config files, etc.
* Maven, for compiling and packaging Java
Future releases will probably include a C++ compiler and other tools.

The server is meant to be a devops platform, available to members of a project team, and separate from the development environment.  The tool set is intended to automate the build out process to a degree where it becomes quick and reliable.

The instructions that follow assume that the server will live in the Amazon AWS cloud environment and that it will be running Amazon Linux.  However, any Linux server from an Amazon competitor that has the yum package manager should work with (hopefully) minor adjustments.  The devops server does not have to be up continuously; it can be shut down when it isn't needed, thus making it very economical for a cost-conscious project.  Another cost-saving measure is to forego a permanent public IP address for the server and let AWS assign one from its pool.  The one drawback of using the pool IP addresses is that they change each time a server is sotpped and started again.  

## Requirements
1. An Amazon AWS instance running Amazon Linux, its public IP address, and the SSH key for accessing the instance. The security group for the instance should allow incoming connections on ports 22 and 8080.
2. A local computer for running the Ansible playbook.  The machine should have Linux (or Mac OSX), the Ansible package, and Git.

## Build Procedure
1. Download this repo to your local computer:
```
      git clone http://github.com/tomlamphier/ansible-devops
```
2. Change the directory to ansible-devops. Copy in SSH key as devops-private-key.pem. Ensure that the permissions are set to 400.
3. Edit the two files ***connect*** and ***hosts***, replacing xxx.xxx.xxx.xxx with the public IP address of the AWS instance.
4. Test your connectivity to the AWS server:
```
      ./connect
      # respond 'yes' to the enevitable authentication message
```
5. Run the Ansible playbook
```
      ansible-playbook -i hosts playbook.yml
      # Note: The playbook displays a Jenkins initial password 
      # near the end of the run. Cut and paste this password to a temp 
      # file for later use.
```
## Post-Build Configuration
The last step of the above build starts Jenkins on the devops server.  We will log on to Jenkins from a browser to perform the post-build configuration:
1. Go to the devops public IP address + port 8080 in a browser.  You will see a page asking for the initial Jenkins password.  Paste it into the appropriate field.  
i2. Select the option to install the recommended plugins. Follow the instructions to create an admin user and password, continue to home page.
3. Go to Manage Jenkins ==> Manage Plugins.  Install the Publish over SSH plugin.  (Install without restart)
4. Go to Manage Jenkins ==> Global Tool Configuration. Scroll down to Maven - Maven installations, click [Add Maven], Set name to "Maven on Devops", uncheck "Install Automatically", set Maven Home to "/usr/local/apache-maven-3.5.2". Click on [Save].  
