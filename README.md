# ansible-devops
This repo builds a server that will have the follwing software:
* Ansible, for installing and configuring software on other servers.
* Jenkins, for continuous project builds and deployment.
* Git, for managing programs, scripts, config files, etc.
* Maven, for compiling and packaging Java
Future releases will probably include a C++ compiler and other tools.

Instructions for deploying the server can be found in [this article](http://datasciex.com/?p=260) on the web.

The server is meant to be a devops platform, available to members of a project team, and separate from the development environment.  The tool set is intended to automate the build out process to a degree where it becomes quick and reliable.

The Ansible code in this repo is tailored to a server that will live in the Amazon AWS cloud environment and that will be running Amazon Linux.  However, any Linux server from an Amazon competitor that has the yum package manager should work with (hopefully) minor adjustments.  The devops server does not have to be up continuously; it can be shut down when it isn't needed, thus making it very economical for a cost-conscious project.  Another cost-saving measure is to forego a permanent public IP address for the server and let AWS assign one from its pool.  The one drawback of using the pool IP addresses is that they change each time a server is sotpped and started again.  
