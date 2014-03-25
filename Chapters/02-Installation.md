#**Lab 2: Installing OpenShift Enterprise 2.0**

**Server used:**

* broker host

**Tools used:**

* openshift.sh

**Note:  For this lab, use the 209.x.x.x IP address when defining your hosts**

##**Overview of *openshift.sh***

The OpenShift team has developed an installation script for the platform that simplifies the installation and configuration of the PaaS.  The *openshift.sh* script is a flexible tool to get an environment up and running quickly without having to worry about manually configuring all of the required services.  For a better understanding of how the tool works, it is suggested that the user view the source code of *openshift.sh* to become familiar with the configuration involved in setting up the platform.  For this training class, once the installation of the PaaS has started, the instructor will go over the architecture of the PaaS so that you are familiar with all of the components and their purpose.

A copy of *openshift.sh* has already been loaded on each system provided to you.  This script is also available in the [enterprise-2.0 branch of the openshift-extras Github repository](https://github.com/openshift/openshift-extras).  In that repository, you can find a [kickstart version](https://github.com/openshift/openshift-extras/blob/enterprise-2.0/enterprise/install-scripts/openshift.ks) of the script as well as other versions.  The script we will be using is the [generic openshift.sh](https://github.com/openshift/openshift-extras/blob/enterprise-2.0/enterprise/install-scripts/generic/openshift.sh) script.

##**Installing and Configuring the OpenShift Broker host using *openshift.sh***

The *openshift.sh* script takes arguments, in the form of environment variables or command-line arguments, to specify settings for the script.  All of the recognized settings are documented extensively in comments at the top of the script.  For our purposes, we will want to specify the following settings when we use this script to install the broker host:

* *install_components=broker,named,activemq,datastore*
* *domain=apps.example.com*
* *hosts_domain=hosts.example.com*
* *broker_hostname=broker.hosts.example.com*
* *named_ip_addr={host1 IP address}*
* *named_entries=named_entries=broker:{host1 IP address},activemq:{host1 IP address},datastore:{host1 IP address},node:{host2 IP address}*

Let's go over each of these options in more detail.

### *install_components* ###

The *install_components* setting specifies which components the script will install and configure.  In this training session, we will install the OpenShift broker and supporting services on which OpenShift depends on one host, and we will install the OpenShift node component on a second host.

In more complex installations, you will want to install each component on a separate host (in fact, you will want to install most components on several hosts each for redundancy).  For testing or POCs, you may want to install all components (including both the OpenShift broker and node) on a single host, which is the default if you do not specify a setting for install_components.

### *domain*, *host_domain*, *broker_hostname*, *named_ip_addr*, and *named_entries* ###

The *domain* setting specifies the domain name that you would like to use for your applications that will be hosted on the OpenShift Enterprise Platform.  The default is example.com, but it makes more sense from an architecture view point to separate these out to their own domain.

The *hosts_domain* setting specifies the domain name that you would like the OpenShift infrastructure hosts to use, which includes the OpenShift broker and node hosts.  This domain also includes hosts running supporting services such as ActiveMQ and MongoDB, although in our case we are running these services on the OpenShift broker host as well.

While it is not required to do so, it is good practice to put your infrastructure hosts (broker and nodes) under a separate domain from applications.

The *broker_hostname* setting specifies the fully-qualified hostname that the installation script will configure for the OpenShift broker host.  In a more complex configuration with redundant brokers, you will want to use this setting to specify a unique hostname for each host that you install (e.g.,* broker_hostname=broker01.hosts.example.com* on the first host, *broker_hostname=broker02.hosts.example.com* on the second host, and so on).  For this training session, we will only be installing one broker host.

For the *named_ip_addr* setting, use the 209.x.x.x address of *host1* that was provided to you by your instructor.  We will be installing our nameserver alongside the OpenShift broker on this host, so we want to make sure that we configure the host to use itself as its own nameserver.  The *named_entries* is used when the host installs the nameserver to add DNS records for the various hosts and services we will be installing.  We tell the installation script to create records with the public-facing IP addresses for these hosts (as opposed to the private, internal IP addresses).

### Executing *openshift.sh* ###

Let's go ahead and execute the command to run installation script.

For own use, set the *host1* and *host2* environment variables:

**Note:** Execute the following command on the broker host and ensure that you replace {host1 IP address} with the broker IP address and {host2 IP address} with the correct node IP address provided to you by the instructor.

	# host1={host1 IP address}; host2={host2 IP address}

For example, if the instructor gave me the following information:
* Broker IP Address = 209.132.179.41
* Node IP Address = 209.132.179.75

I would enter in the following command:

	# host1=209.132.179.41; host2=209.132.179.75

Next, for *openshift.sh*, set the following environment variables:

**Note:** Perform the following command on the broker host.

	export CONF_INSTALL_COMPONENTS=broker,named,activemq,datastore
	export CONF_DOMAIN=apps.example.com
	export CONF_HOSTS_DOMAIN=hosts.example.com
	export CONF_BROKER_HOSTNAME=broker.hosts.example.com
	export CONF_NAMED_IP_ADDR=$host1
	export CONF_NAMED_ENTRIES=broker:$host1,activemq:$host1,datastore:$host1,node:$host2

While we could use the corresponding command-line arguments to specify these settings, we will use environment variables to force ourselves to be careful.  After setting the above variables, run the following command and verify that the variables have all been set as described above, with IP addresses substituted appropriately:

	env | grep CONF_

**Note:** Verify that the settings are correct.

Now let's execute *openshift.sh*.  Note that we will use the *tee* command to make an installation log.

**Note:** Perform the following command on the broker host.

	# sh openshift.sh |& tee broker-install.log

The installation script will take a while depending on the speed of the connection at your location.  While the installation script runs on the OpenShift broker host, open a new terminal window or tab and continue on to the next section to begin the installation and configuration of your second host which will be the node host.

##**Installing and Configuring the OpenShift Node host**

To install and configure the OpenShift node host, we will want to specify the following settings to *openshift.sh*:

* *install_components=node*
* *cartridges=all,-jboss,-jenkins,-postgres,-diy*
* *domain=apps.example.com*
* *hosts_domain=hosts.example.com*
* *named_ip_addr={host1 IP address}*
* *node_hostname=node.hosts.example.com*
* *node_ip_addr={host2 IP address}*

Following is an explanation for each of these arguments.

### *install_components* and *cartridges* ###

We are configuring this host as an OpenShift node host.  As the instructor will explain shortly in the lecture, a node has several "cartridges" installed, which provide language runtimes, Web frameworks, databases, and other features for application developers to use.  For now, we will install all available cartridges except for JBoss Web frameworks, the Jenkins continuous integration environment, the PostgreSQL DBMS, and the DIY cartridge.

### *domain*, *hosts_domain*, *named_ip_addr*, *node_hostname*, and *node_ip_addr* ###

We described the *domain* and *hosts_domain* settings earlier in this lab while installing and configuring the OpenShift broker host.

For the *named_ip_addr* setting, use the 209.x.x.x address for *host1* that was provided to you by your instructor. We use the *named_ip_addr* setting to configure name resolution on the OpenShift node host to use our own nameserver.

The *node_hostname* setting specifies the fully-qualified hostname that the installation script will configure for the OpenShift node host.

For the *named_ip_addr* setting, use the 209.x.x.x address for *host2* that was provided to you by your instructor. We use the *node_ip_addr* setting to tell the installation script to configure this OpenShift node host to use its public-facing IP address when it configures routing rules for user applications.

### Executing *openshift.sh* ###

Before we execute the command to run installation script, let's set the *host1* and *host2* environment variables as we did on the broker host:

**Note:** Perform the following command on the node host.

	# host1={host1 IP address}; host2={host2 IP address}

For example, if the instructor gave me the following information:
* Broker IP Address = 209.132.179.41
* Node IP Address = 209.132.179.75

**Note:** Perform the following command on the node host.

I would enter in the following command:

	# host1=209.132.179.41; host2=209.132.179.75

Next, set the following environment variables:

	export CONF_INSTALL_COMPONENTS=node
	export CONF_CARTRIDGES=all,-jboss,-jenkins,-postgres,-diy
	export CONF_DOMAIN=apps.example.com
	export CONF_HOSTS_DOMAIN=hosts.example.com
	export CONF_NAMED_IP_ADDR=$host1
	export CONF_NODE_HOSTNAME=node.hosts.example.com
	export CONF_NODE_IP_ADDR=$host2

Run the following command and verify that the settings are correct, with the appropriate IP addresses substituted:

	env | grep CONF_

**Note:** Verify that the settings are correct.

Now launch the installation script:

	# sh openshift.sh |& tee node-install.log

The installation script will take a while depending on the speed of the connection at your location and the number of RPM packages that need to be installed.  During this time, the instructor will lecture about the architecture of OpenShift Enterprise.

**Lab 2 Complete!**

<!--BREAK-->
