# Implemnting a Full CI/CD Pipeline

## SCM

### Git

To clone a remote repo:

    git clone <repo url>

To initialize a remote repo:

    git init

Query state of repo:

    git status

To stage changed file for next commit:

    git add <file>

To add all file to the stage:

    git add -A

To commit staged changes to local repo:

    git commit -m "<message>"

To push changes to remote repo:

    git push

To push and specify the remote repo and remote branch:

    git push -u <remote name,usually origin> <branch name>

To show a list of branches in current repository:

    git branch

To check out an existing branch:

    git checkout <branch>

To create a new branch and check it out:

    git checkout -b <new branch>

To tag a new tag pointing to the current latest commit:

    git tag <tagName>

___

## Build Automation

<p>
Build automation is the automation of tasks needed for processing and preparing source code for deployment to production.
</p>
Tasks like:

* Compiling
* Dependency Management
* Executing Automated Tests
* Packaging the Application for Deployment

### Gradle

<p>
Gradle Wrapper: Is a script that invokes a set version of Gradle, and Downloads it. Only requires Java.
</p>

To initialize a new project:

    gradle init

<p> the 'build.gradle' file controls what tasks are available for the project. Tasks can depend on another and depending tasks will be run before as well.</p>

To call tasks:

    ./gradlew someTask anotherTask

To set dependencies in build.gradle:

    taskA.dependsOn taskB

To include plugins in build.gradle:

    plugins{
        id"<plugin id>"version"<plugin varsion>"

    }

Example of task definition in build.gradle:

    task sayHello << {
        println 'Hello, World!'
    }

    task anotherTask << {
        println 'This is another task'
    }

    sayHello.dependsOn anotherTask

### Automated Tests

<p>
It is automated execution of tests, often running as part of the build process that verify the quality and stability of code.
</p>

Types:

* Unit Tests: testing on small pieces of code in isolation, usually a single method or function.

* Integration Tests: testing larger portions of an application that are integrated with each other.

* Smoke Tests / Sanity Tests: high level integration tests that verify basic large-scale things like if the application runs or return errors or...

___

## Continuous Integration

<p>
It is the practice of frequently merging code changes. To acheive this, when a change in code is detected in source control, the CI server detects the code change and executes a build that automatically prepares/compiles the code and runs automated tests. This gives us the benefit of immediate feedback in case of something being broken.
</p>

### Jenkins

<p>
Some versions of CentOS might have a java version that is not compatible with Jenkins so It's better to:
</p>

    sudo yum remove java
    sudo yum install java-1.8.0-openjdk

To install Jenkins:
    sudo yum install epel-release
    sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
    sudo rpm --import https://jenkins-ci.org/redhat/jenkins-ci.org.key

    sudo yum -y install jenkins-2.121.1

To enable and start the jenkins service:

    sudo systemctl enable Jenkins
    sudo systemctl start jenkins

To access jenkins setup using browser:

    <server IP>:8080 # Follow the screen prompts.