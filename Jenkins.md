
# Jenkins

## History and Usage
<p>In the classic method to develop software,  </p>

## Concepts

### CI

<p> A software development practice in which members of a team integrate their work frequently, at least daily, leading to multiple integrations per day. </p>

### CD

<p> A software development discipline where software is built in a manner that allows for deploying to customers at any time.</p>

### Jobs in Jenkins

<p> Any runnable task that is controlled by Jenkins. Job=Project.</p> 

- Freestyle Project: The most common type of project. The build step normally executes a shell command. Most freedom in configuration.
- Pipeline: Normally writne in DSL. Use for two complicated or projects that span multiple nodes. For projects with large scope.
- Multi-Configuration Project: For projects that are tested on multiple environments, android windows linux ...
- Git-Hub Organization: Use the source control platform's org and allows Jenkins to act on Jenkinsfiles stored within organization's repositories.
- Folder: An item not a project. Creates a folder hierarchy for giving logical organization for items. The folder name becomes a part of the path for the project.
- Multi-Branch Pipeline: Jenkins uses a Jenkinsfile to mark repositories. If a branch is created, Jenkins will make a new proj in Jenkins for that project.

### Builds in Jenkins
<p> A Build is the result of a single execution of a project. It will produce artifacts, which are immutable files (jar, war, config files and other generated assets.) that are generated during a build or a pipeline run. They are archived on the Jenkins master for later retrieval. These artifacts are maintained in a repo on Jenkins master or in an another SCM. Repositories hold items that need to be retrieved, such as source code, compiled code artifacts and config files.

### Build Tools
<p> Software that actually performs the build portion of the pipeline, like Maven, Ant, and shell scripting. Usually installed as plugins.</p>

### Source Code Manager 
<p>A SCM is a Software that track changes in code. Changes in code(revisions) are timestamped and include the identity of the person that made the change. These changes can be tracked or roll backed. Versions can be compared, stored and merged with other versions. Examples are Git, Subversion, Mercurial and Perforce.

### Testing
<p> The process of checking code to ensure that it is working as designed, or that its output is what expected.</p>

- Unit Test: Individual components(units) like classes, methods and modules are tested to ensure that outputs are as expected.

- Smoke Test: A general test of the software for checking its main functionality. To see if it's stable enough for further testing. Does it load? Does the menus work? Does it crash when the save button is clicked?

- Verification Test: Did we satisfy the build requirements as mentioned in the build doc? Automated verification testing is used to streamline this process.

- Functional Test: Checks a specific, probably recently added function of the software. Does this feature work? Can a user do this?

- Acceptance Test: We handoff the software to the client and the client does the acceptance test to ensure that the software meets their expectations.

### Notifications
<p> Notifications give feedback to the status of processes within the project. They are critical to an automated process. If a build fails, or if it's needed to manually approve a deployment, a notification can be configured to be sent. They can be in form of emails, sms and several types of instant messaging platforms like slack that are configurable using plugins.</p>

### Distributed Builds
<p> Distributed builds are "build jobs" in which the executor of the build is located on an agent (node) that is separate from the master. The reason for this is parallelism. The master acts as the controller for the build, running specific builds on specific agents, which results in parallelism and greater ease in multiconfiguration pipelines. So for 3 versions of a software to do 5 unit tests against, this can be done in one parallel pass, to have 5 tests on 3 agents, instead of 15 tests on the master. Nodes with specific configurations can be tagged so that pipeline steps specific to that configuration are directed to that node. Agents must be "fungible", Which means replacable, as in there should be nothing special about them and the local configuration should be kept to minimal and the global configurations  on the master should be preferred.</p>

### Plugins
<p> Extensions to Jenkins that add to its functionality.</p>

### Security
<p>Jenkins uses Matrix Security for authorization.</p>

### Artifacts

### Fingerprints


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

### Centos7

    sudo su # this prevents us from having to issue sudo each time.  
    yum install -y wget  
    wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo  
    rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key  
    yum install jenkins  java-1.8.0-openjdk-devel git -y  
    systemctl enable jenkins && systemctl restart jenkins 


## Configurations

* The default location of the archive repo:
    Jenkins root/jobs/buildname/builds/lastSuccessfulBuild/archive


## HA

## Fault Tolerance

## Backup and Restore

## Monitoring

## Performance Tuning