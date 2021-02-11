
# Jenkins

## History and Usage
<p>In the classic method to develop software </p>

## Concepts

## Architecture

* Source: https://devopscube.com/jenkins-architecture-explained/

![Jenkins Architecture](https://devopscube.com/wp-content/uploads/2020/03/jenkins-architecture-1024x657.png)

### Key Components
1. Jenkins Master Node
2. Jenkins Slave Node
3. Jenkins Web Interface

### Jenkins Master (Server)

* Jenkins’s server or master node holds all key configurations.

* The following are the key Jenkins master components:

<p> Jenkins Jobs: A job is a collection of steps that you can use to build your source code, test your code, run a shell script, or to run an Ansible role in a remote host. Ther are multiple job types available to support your workflow for continuous integration & continuous delivery. </p>

<p>Jenkins Plugins: Plugins are community-developed modules that you can install in your Jenkins server. It lets you add more functionalities that are not natively available in Jenkins. You can also develop your custom plugins. Check out all plugins from the Jenkins Plugin Index </p>

<p>Jenkins User: Jenkins has its own user database. It can be used for Jenkins’s authentication.</p>

<p>Jenkins Global Security: Jenkins has the following two primary authentication methods.</p>

1. Jenkins’s own user database:- Set of users maintained by Jenkins’s own database.
2. LDAP Integration:- Jenkins authentication using corporate LDAP configuration. 

<p>Jenkins Credentials: If you want to save any secret information that has to be used in the jobs, you can store it as a credential. All credentials are encrypted by Jenkins.</p>

<p>Jenkins Nodes/Clouds: You can configure multiple slave nodes (Linux/Windows) or clouds (docker, kubernetes) for executing Jenkins jobs.</p>

<p>Jenkins Global Settings (Configure System): Under Jenkins global configuration, you have all the configurations of installed plugins and native Jenkins global configurations. Also, you can configure global environment variables under this section.</p>

<p>Jenkins Logs: Provides logging information on all Jenkins server actions including job logs, plugin logs, webhook logs, etc.</p>

<p>All the configurations for the above-mentioned components will be present as a config file in the Jenkins master node.</p>

* Note: Jenkins doesn’t have a database. All Jenkins configurations are stored as flat config files. Mostly xml files.

### Jenkins Slave

* Jenkins slaves are the worker nodes for the jobs configured in Jenkins server.

<p>Note: You can run jobs in Jenkins server without a Jenkins slave. However, the recommended approach is to have segregated Jenkins Slaves for different job requirements so that you don’t end up messing up the Jenkins server for any system wide configuration changes required for a job.</p>

<p>You can have any number of Jenkins slaves attached to a master with a combination of Windows & Linux servers.</p>

<p>Also, you can restrict jobs to run on specific slaves, depending on the use case. For example, if you have a slave with java 8 configurations, you can assign this slave for jobs that require Java 8 environment.</p>

<p>There is no single standard for using the slaves. You can set up a workflow and strategy based on your project needs.</p>

### Jenkins Web Interface

<p>Jenkins 2.0 introduced a very intuitive web interface called “Jenkins Blue Ocean”. It has a good visual representation of all the pipelines.</p>

![Blue Ocean Architecture](https://devopscube.com/wp-content/uploads/2020/03/jenkins-blue-ocean.png)

Jenkins Master-Slave Connectivity
You can connect a Jenkins master and slave in two ways

1. Using the SSH method: Uses the ssh protocol to connect to the slave. The connection gets initiated from the Jenkins master. There should be connectivity over port 22 between master and slave.

2. Using the JNLP method: Uses java JNLP protocol. In this method, a java agent gets initiated from the slave with Jenkins master details. For this, the master nodes firewall should allow connectivity on specified JNLP port. Typically the port assigned will be 50000. This value is configurable.

### Jenkins Master-Slave Connectivity
* You can connect a Jenkins master and slave in two ways

1. Using the SSH method: Uses the ssh protocol to connect to the slave. The connection gets initiated from the Jenkins master. Ther should be connectivity over port 22 between master and slave.

2. Using the JNLP method: Uses java JNLP protocol. In this method, a java agent gets initiated from the slave with Jenkins master details. For this, the master nodes firewall should allow connectivity on specified JNLP port. Typically the port assigned will be 50000. This value is configurable.

* There are two types of Jenkins slaves

1. Slave Nodes: These are servers (Windows/Linux) that will be configured as static slaves. These slaves will be up and running all the time and stay connected to the Jenkins server. Organizations use custom scripts to shut down and restart the slaves when is not used. Typically during nights & weekends.

2. Slave Clouds: Jenkins Cloud slave is a concept of having dynamic slaves. Means, whenever you trigger a job, a slave will be deployed as a VM/container on demand and gets deleted once the job is completed. This method saves money in terms of infra cost when you have a huge Jenkins ecosystem and continuous builds.

### Jenkins Data
<p> All the Jenkins data will be store in the following folder location. Data includes all jobs config files, plugins configs, secrets, node information, etc. </p>

    /var/lib/jenkins/

* It is very important to back up the Jenkins data folder every day. For some reason, if your Jenkins server data gets corrupt, you can restore whole Jenkins with the data backup.




## Installation

### Running Jenkins from War File

    sudo yum install -y openjdk-8-jre
    wget http://mirrors.jenkins.io/war-stable/latest/jenkins.war
    java -jar jenkins.war --httpPort=80 --prefix=/dashboard

### 




## Configurations

## HA

## Fault Tolerance

## Backup and Restore

## Monitoring

## Performance Tuning